# Learning Center Platform - Part 5

## Table of contents

## Learning Bounded Context

### Package application.internal outboundservices.acl

En el package `acl` crear la clase `ExternalProfileService` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.ProfileId;
import pe.edu.upc.center.platform.profiles.interfaces.acl.ProfilesContextFacade;

import java.util.Optional;

/**
 * ExternalProfileService
 *
 * <p>
 *     This class is an outbound service used by the Learning Context to interact with the Profiles Context.
 *     It is implemented as part of an anti-corruption layer (ACL) to decouple the Learning Context from the Profiles Context.
 * </p>
 *
 */
@Service
public class ExternalProfileService {

  private final ProfilesContextFacade profilesContextFacade;

  public ExternalProfileService(ProfilesContextFacade profilesContextFacade) {
    this.profilesContextFacade = profilesContextFacade;
  }

  /**
   * Fetch profileId by email
   *
   * @param email the email to search for
   * @return profileId if found, empty otherwise
   */
  public Optional<ProfileId> fetchProfileIdByEmail(String email) {
    var profileId = profilesContextFacade.fetchProfileIdByEmail(email);
    if (profileId == 0L)
      return Optional.empty();
    return Optional.of(new ProfileId(profileId));
  }

  /**
   * Create profile
   *
   * @param firstName the first name
   * @param lastName the last name
   * @param email the email
   * @param street the street address
   * @param number the number
   * @param city the city
   * @param state the state
   * @param zipCode the zip code
   * @return profileId if created, empty otherwise
   */
  public Optional<ProfileId> createProfile(String firstName, String lastName, String email, String street, 
                                           String number, String city, String state, String zipCode) {
    var profileId = profilesContextFacade.createProfile(firstName, lastName, email, street, number, city, 
            state, zipCode);
    if (profileId == 0L)
      return Optional.empty();
    return Optional.of(new ProfileId(profileId));
  }
}
```

### Package application.internal commandservices

En el package `commandservices` reemplazar el contenido de la clase `StudentCommandServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.learning.application.internal.outboundservices.acl.ExternalProfileService;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Student;
import pe.edu.upc.center.platform.learning.domain.model.commands.CreateStudentCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.UpdateStudentMetricsOnTutorialCompletedCommand;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.AcmeStudentRecordId;
import pe.edu.upc.center.platform.learning.domain.services.StudentCommandService;
import pe.edu.upc.center.platform.learning.infrastructure.persistence.jpa.repositories.StudentRepository;

/**
 * Implementation of StudentCommandService
 *
 * <p>
 *     This class is the implementation of the StudentCommandService interface.
 *     It is used by the LearningContext to handle commands on the Student aggregate.
 * </p>
 *
 */
@Service
public class StudentCommandServiceImpl implements StudentCommandService {
  private final StudentRepository studentRepository;
  private final ExternalProfileService externalProfileService;

  public StudentCommandServiceImpl(StudentRepository studentRepository, ExternalProfileService externalProfileService) {
    this.studentRepository = studentRepository;
    this.externalProfileService = externalProfileService;
  }

  //**
  // * Command handler to create student
  // *
  // * @param command containing student details
  // * @return AcmeStudentRecordId
  // */
  @Override
  public AcmeStudentRecordId handle(CreateStudentCommand command) {

    // Fetch profileId by email
    var profileId = externalProfileService.fetchProfileIdByEmail(command.email());

    // If profileId is empty, create profile
    if (profileId.isEmpty()) {
      profileId = externalProfileService.createProfile(command.firstName(), command.lastName(), command.email(), command.street(), command.number(), command.city(), command.postalCode(), command.country());
    } else {
      // If profileId is not empty, check if student exists
      studentRepository.findByProfileId(profileId.get()).ifPresent(student -> {
        throw new IllegalArgumentException("Student already exists");
      });
    }

    // If profileId is still empty, throw exception
    if (profileId.isEmpty())
      throw new IllegalArgumentException("Unable to create profile");

    // Create student using fetched or created profileId
    var student = new Student(profileId.get());
    studentRepository.save(student);
    return student.getAcmeStudentRecordId();
  }

  //**
  // * Command handler to update student metrics on tutorial completed
  // *
  // * @param command containing studentRecordId
  // * @return AcmeStudentRecordId
  // */
  @Override
  public AcmeStudentRecordId handle(UpdateStudentMetricsOnTutorialCompletedCommand command) {
    studentRepository.findByAcmeStudentRecordId(command.studentRecordId()).map(student -> {
      // Update student metrics
      student.updateMetricsOnTutorialCompleted();
      studentRepository.save(student);
      return student.getAcmeStudentRecordId();
    }).orElseThrow(() -> new IllegalArgumentException("Student with given Id not found"));
    return null;
  }
}
```

