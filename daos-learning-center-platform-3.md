# Learning Center Platform - Part 3

## Table of contents

## Learning Bounded Context

### Package domain . model . queries

En el package `queries` crear el record `GetAllCoursesQuery` con el siguiente contenido:
```java
public record GetAllCoursesQuery() {
}
```

En el package `queries` crear el record `GetAllEnrollmentsByAcmeStudentRecordIdQuery` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.AcmeStudentRecordId;

public record GetAllEnrollmentsByAcmeStudentRecordIdQuery(AcmeStudentRecordId studentRecordId) {
}
```

En el package `queries` crear el record `GetAllEnrollmentsByCourseIdQuery` con el siguiente contenido:
```java
public record GetAllEnrollmentsByCourseIdQuery(Long courseId) {
}
```

En el package `queries` crear el record `GetAllEnrollmentsQuery` con el siguiente contenido:
```java
public record GetAllEnrollmentsQuery() {
}
```

En el package `queries` crear el record `GetCourseByIdQuery` con el siguiente contenido:
```java
public record GetCourseByIdQuery(Long courseId) {
}
```

En el package `queries` crear el record `GetEnrollmentByAcmeStudentRecordIdAndCourseIdQuery` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.AcmeStudentRecordId;

public record GetEnrollmentByAcmeStudentRecordIdAndCourseIdQuery(AcmeStudentRecordId acmeStudentRecordId, Long courseId) {
}
```

En el package `queries` crear el record `GetEnrollmentByIdQuery` con el siguiente contenido:
```java
public record GetEnrollmentByIdQuery(Long enrollmentId) {
}
```

En el package `queries` crear el record `GetLearningPathItemByCourseIdAndTutorialIdQuery` con el siguiente contenido:
```java
public record GetLearningPathItemByCourseIdAndTutorialIdQuery(Long courseId, Long tutorialId) {
}
```

En el package `queries` crear el record `GetStudentByAcmeStudentRecordIdQuery` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.AcmeStudentRecordId;

public record GetStudentByAcmeStudentRecordIdQuery(AcmeStudentRecordId acmeStudentRecordId) {
}
```

En el package `queries` crear el record `GetStudentByProfileIdQuery` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.ProfileId;

public record GetStudentByProfileIdQuery(ProfileId profileId) {
}
```

### Package domain . model . commands

En el package `commands` crear el record `AddTutorialToCourseLearningPathCommand` con el siguiente contenido:
```java
public record AddTutorialToCourseLearningPathCommand(Long tutorialId, Long courseId) {
}
```

En el package `commands` crear el record `CancelEnrollmentCommand` con el siguiente contenido:
```java
public record CancelEnrollmentCommand(Long enrollmentId) {
}
```

En el package `commands` crear el record `CompleteTutorialForEnrollmentCommand` con el siguiente contenido:
```java
public record CompleteTutorialForEnrollmentCommand(Long enrollmentId, Long tutorialId) {
}
```

En el package `commands` crear el record `ConfirmEnrollmentCommand` con el siguiente contenido:
```java
public record ConfirmEnrollmentCommand(Long enrollmentId) {
}
```

En el package `commands` crear el record `CreateCourseCommand` con el siguiente contenido:
```java
public record CreateCourseCommand(String title, String description) {
}
```

En el package `commands` crear el record `CreateStudentCommand` con el siguiente contenido:
```java
public record CreateStudentCommand(String firstName, String lastName, String email, String street, String number, String city, String postalCode, String country) {
}
```

En el package `commands` crear el record `DeleteCourseCommand` con el siguiente contenido:
```java
public record DeleteCourseCommand(Long courseId) {
}
```

En el package `commands` crear el record `RejectEnrollmentCommand` con el siguiente contenido:
```java
public record RejectEnrollmentCommand(Long enrollmentId) {
}
```

En el package `commands` crear el record `RequestEnrollmentCommand` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.AcmeStudentRecordId;

public record RequestEnrollmentCommand(AcmeStudentRecordId studentRecordId, Long courseId) {
}
```

En el package `commands` crear el record `UpdateCourseCommand` con el siguiente contenido:
```java
public record UpdateCourseCommand(Long id, String title, String description) {
}
```

En el package `commands` crear el record `UpdateStudentMetricsOnTutorialCompletedCommand` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.AcmeStudentRecordId;

public record UpdateStudentMetricsOnTutorialCompletedCommand(AcmeStudentRecordId studentRecordId) {
}
```

### Package domain . services

En el package `services` crear el interface `CourseCommandService` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Course;
import pe.edu.upc.center.platform.learning.domain.model.commands.AddTutorialToCourseLearningPathCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.CreateCourseCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.DeleteCourseCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.UpdateCourseCommand;

import java.util.Optional;

public interface CourseCommandService {
  Long handle(CreateCourseCommand command);
  Optional<Course> handle(UpdateCourseCommand command);
  void handle(DeleteCourseCommand command);

  void handle(AddTutorialToCourseLearningPathCommand command);
}
```

En el package `services` crear el interface `CourseQueryService` con el siguiente contenido:

```java
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Course;
import pe.edu.upc.center.platform.learning.domain.model.entities.LearningPathItem;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetAllCoursesQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetCourseByIdQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetLearningPathItemByCourseIdAndTutorialIdQuery;

import java.util.List;
import java.util.Optional;

public interface CourseQueryService {
  Optional<Course> handle(GetCourseByIdQuery query);
  List<Course> handle(GetAllCoursesQuery query);
  Optional<LearningPathItem> handle(GetLearningPathItemByCourseIdAndTutorialIdQuery query);
}
```

En el package `services` crear el interface `EnrollmentCommandService` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.commands.*;

public interface EnrollmentCommandService {
  Long handle(RequestEnrollmentCommand command);
  Long handle(ConfirmEnrollmentCommand command);
  Long handle(RejectEnrollmentCommand command);

  Long handle(CancelEnrollmentCommand command);

  Long handle(CompleteTutorialForEnrollmentCommand command);
}
```

En el package `services` crear el interface `EnrollmentQueryService` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Enrollment;
import pe.edu.upc.center.platform.learning.domain.model.queries.*;

import java.util.List;
import java.util.Optional;

public interface EnrollmentQueryService {
  List<Enrollment> handle(GetAllEnrollmentsByAcmeStudentRecordIdQuery query);
  Optional<Enrollment> handle(GetEnrollmentByIdQuery query);
  List<Enrollment> handle(GetAllEnrollmentsQuery query);
  List<Enrollment> handle(GetAllEnrollmentsByCourseIdQuery query);
  Optional<Enrollment> handle(GetEnrollmentByAcmeStudentRecordIdAndCourseIdQuery query);
}
```

En el package `services` crear el interface `StudentCommandService` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.commands.CreateStudentCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.UpdateStudentMetricsOnTutorialCompletedCommand;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.AcmeStudentRecordId;

public interface StudentCommandService {
  AcmeStudentRecordId handle(CreateStudentCommand command);
  AcmeStudentRecordId handle(UpdateStudentMetricsOnTutorialCompletedCommand command);
}
```

En el package `services` crear el interface `StudentQueryService` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Student;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetStudentByAcmeStudentRecordIdQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetStudentByProfileIdQuery;

import java.util.Optional;

public interface StudentQueryService {
  Optional<Student> handle(GetStudentByProfileIdQuery query);
  Optional<Student> handle(GetStudentByAcmeStudentRecordIdQuery query);
}
```

### Package domain . exceptions

En el package `exceptions` crear el interface `CourseNotFoundException` con el siguiente contenido:
```java
public class CourseNotFoundException extends RuntimeException {
  public CourseNotFoundException(Long aLong) {
    super("Course with id " + aLong + " not found");
  }
}
```

### Package infrastructure.persistence.jpa.repositories

En el package `repositories` crear el interface `CourseRepository` con el siguiente contenido:
```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Course;

import java.util.Optional;

@Repository
public interface CourseRepository extends JpaRepository<Course, Long> {
  Optional<Course> findByTitle(String title);
  boolean existsByTitle(String title);
  boolean existsByTitleAndIdIsNot(String title, Long id);
}
```

En el package `repositories` crear el interface `EnrollmentRepository` con el siguiente contenido:
```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Enrollment;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.AcmeStudentRecordId;

import java.util.List;
import java.util.Optional;

@Repository
public interface EnrollmentRepository extends JpaRepository<Enrollment, Long> {
  List<Enrollment> findAllByAcmeStudentRecordId(AcmeStudentRecordId acmeStudentRecordId);
  List<Enrollment> findAllByCourseId(Long courseId);
  Optional<Enrollment> findByAcmeStudentRecordIdAndCourseId(AcmeStudentRecordId acmeStudentRecordId, Long courseId);
}
```

En el package `repositories` crear el interface `StudentRepository` con el siguiente contenido:
```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Student;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.AcmeStudentRecordId;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.ProfileId;

import java.util.Optional;

@Repository
public interface StudentRepository extends JpaRepository<Student, Long> {
  Optional<Student> findByAcmeStudentRecordId(AcmeStudentRecordId studentRecordId);
  Optional<Student> findByProfileId(ProfileId profileId);
}
```

### Package application.internal queryservices

En el package `queryservices` crear la clase `CourseQueryServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Course;
import pe.edu.upc.center.platform.learning.domain.model.entities.LearningPathItem;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetAllCoursesQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetCourseByIdQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetLearningPathItemByCourseIdAndTutorialIdQuery;
import pe.edu.upc.center.platform.learning.domain.services.CourseQueryService;
import pe.edu.upc.center.platform.learning.infrastructure.persistence.jpa.repositories.CourseRepository;

import java.util.List;
import java.util.Optional;

@Service
public class CourseQueryServiceImpl implements CourseQueryService {

  private final CourseRepository courseRepository;

  public CourseQueryServiceImpl(CourseRepository courseRepository) {
    this.courseRepository = courseRepository;
  }

  @Override
  public Optional<Course> handle(GetCourseByIdQuery query) {
    return courseRepository.findById(query.courseId());
  }

  @Override
  public List<Course> handle(GetAllCoursesQuery query) {
    return courseRepository.findAll();
  }

  @Override
  public Optional<LearningPathItem> handle(GetLearningPathItemByCourseIdAndTutorialIdQuery query) {
    return courseRepository.findById(query.courseId()).map(course -> course.getLearningPath().getLearningPathItemWithTutorialId(query.tutorialId()));
  }
}
```

En el package `queryservices` crear la clase `EnrollmentQueryServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Enrollment;
import pe.edu.upc.center.platform.learning.domain.model.queries.*;
import pe.edu.upc.center.platform.learning.domain.services.EnrollmentQueryService;
import pe.edu.upc.center.platform.learning.infrastructure.persistence.jpa.repositories.EnrollmentRepository;

import java.util.List;
import java.util.Optional;

/**
 * Implementation of EnrollmentQueryService
 *
 * <p>
 *     This class is the implementation of the EnrollmentQueryService interface.
 *     It is used by the EnrollmentContext to handle queries on the Enrollment aggregate.
 *     It uses the EnrollmentRepository to query the database.
 * </p>
 */
@Service
public class EnrollmentQueryServiceImpl implements EnrollmentQueryService {
  private final EnrollmentRepository enrollmentRepository;

  public EnrollmentQueryServiceImpl(EnrollmentRepository enrollmentRepository) {
    this.enrollmentRepository = enrollmentRepository;
  }

  /**
   * Query handler to get student enrollments
   *
   * @param query containing studentRecordId
   * @return List of Enrollments
   * @see Enrollment
   * @see GetAllEnrollmentsByAcmeStudentRecordIdQuery
   */
  @Override
  public List<Enrollment> handle(GetAllEnrollmentsByAcmeStudentRecordIdQuery query) {
    return enrollmentRepository.findAllByAcmeStudentRecordId(query.studentRecordId());
  }

  /**
   * Query handler to get enrollment by id
   *
   * @param query containing enrollmentId
   * @return Enrollment
   * @see Enrollment
   * @see GetEnrollmentByIdQuery
   */
  @Override
  public Optional<Enrollment> handle(GetEnrollmentByIdQuery query) {
    return enrollmentRepository.findById(query.enrollmentId());
  }

  /**
   * Query handler to get all enrollments
   *
   * @param query containing no parameters
   * @return List of Enrollments
   * @see Enrollment
   * @see GetAllEnrollmentsQuery
   */
  @Override
  public List<Enrollment> handle(GetAllEnrollmentsQuery query) {
    return enrollmentRepository.findAll();
  }

  /**
   * Query handler to get course enrollments
   *
   * @param query containing courseId
   * @return List of Enrollments
   * @see Enrollment
   * @see GetAllEnrollmentsByCourseIdQuery
   */
  @Override
  public List<Enrollment> handle(GetAllEnrollmentsByCourseIdQuery query) {
    return enrollmentRepository.findAllByCourseId(query.courseId());
  }

  @Override
  public Optional<Enrollment> handle(GetEnrollmentByAcmeStudentRecordIdAndCourseIdQuery query) {
    return enrollmentRepository.findByAcmeStudentRecordIdAndCourseId(query.acmeStudentRecordId(), query.courseId());
  }
}
```

En el package `queryservices` crear la clase `StudentQueryServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Student;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetStudentByAcmeStudentRecordIdQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetStudentByProfileIdQuery;
import pe.edu.upc.center.platform.learning.domain.services.StudentQueryService;
import pe.edu.upc.center.platform.learning.infrastructure.persistence.jpa.repositories.StudentRepository;

import java.util.Optional;

/**
 * Implementation of StudentQueryService
 *
 * <p>
 *     This class is the implementation of the StudentQueryService interface.
 *     It is used by the LearningContext to handle queries on the Student aggregate.
 * </p>
 *
 */
@Service
public class StudentQueryServiceImpl implements StudentQueryService {

  private final StudentRepository studentRepository;

  public StudentQueryServiceImpl(StudentRepository studentRepository) {
    this.studentRepository = studentRepository;
  }

  /**
   * Query handler to get student by profileId
   *
   * @param query containing profileId
   * @return Student
   */
  @Override
  public Optional<Student> handle(GetStudentByProfileIdQuery query) {
    return studentRepository.findByProfileId(query.profileId());
  }

  @Override
  public Optional<Student> handle(GetStudentByAcmeStudentRecordIdQuery query) {
    return studentRepository.findByAcmeStudentRecordId(query.acmeStudentRecordId());
  }
}
```

### Package application.internal commandservices

En el package `commandservices` crear la clase `CourseCommandServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Course;
import pe.edu.upc.center.platform.learning.domain.model.commands.AddTutorialToCourseLearningPathCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.CreateCourseCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.DeleteCourseCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.UpdateCourseCommand;
import pe.edu.upc.center.platform.learning.domain.services.CourseCommandService;
import pe.edu.upc.center.platform.learning.infrastructure.persistence.jpa.repositories.CourseRepository;

import java.util.Optional;

@Service
public class CourseCommandServiceImpl implements CourseCommandService {

  private final CourseRepository courseRepository;

  public CourseCommandServiceImpl(CourseRepository courseRepository) {
    this.courseRepository = courseRepository;
  }

  @Override
  public Long handle(CreateCourseCommand command) {
    if (courseRepository.existsByTitle(command.title())) {
      throw new IllegalArgumentException("Course with same title already exists");
    }
    var course = new Course(command);
    try {
      courseRepository.save(course);
    } catch (Exception e) {
      throw new IllegalArgumentException("Error while saving course: " + e.getMessage());
    }
    return course.getId();
  }

  @Override
  public Optional<Course> handle(UpdateCourseCommand command) {
    if (courseRepository.existsByTitleAndIdIsNot(command.title(), command.id()))
      throw new IllegalArgumentException("Course with same title already exists");
    var result = courseRepository.findById(command.id());
    if (result.isEmpty()) 
      throw new IllegalArgumentException("Course does not exist");
    var courseToUpdate = result.get();
    try {
      var updatedCourse = courseRepository.save(courseToUpdate.updateInformation(command.title(), command.description()));
      return Optional.of(updatedCourse);
    } catch (Exception e) {
      throw new IllegalArgumentException("Error while updating course: " + e.getMessage());
    }
  }

  @Override
  public void handle(DeleteCourseCommand command) {
    if (!courseRepository.existsById(command.courseId())) {
      throw new IllegalArgumentException("Course does not exist");
    }
    try {
      courseRepository.deleteById(command.courseId());
    } catch (Exception e) {
      throw new IllegalArgumentException("Error while deleting course: " + e.getMessage());
    }
  }

  @Override
  public void handle(AddTutorialToCourseLearningPathCommand command) {
    if (!courseRepository.existsById(command.courseId())) {
      throw new IllegalArgumentException("Course does not exist");
    }
    try {
      courseRepository.findById(command.courseId()).map(course -> {
        course.addTutorialToLearningPath(command.tutorialId());
        courseRepository.save(course);
        System.out.println("Tutorial added to learning path");
        return course;
      });
    } catch (Exception e) {
      throw new IllegalArgumentException("Error while adding tutorial to learning path: " + e.getMessage());
    }
  }
}
```

En el package `commandservices` crear la clase `EnrollmentCommandServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.learning.domain.exceptions.CourseNotFoundException;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Course;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Enrollment;
import pe.edu.upc.center.platform.learning.domain.model.commands.*;
import pe.edu.upc.center.platform.learning.domain.services.EnrollmentCommandService;
import pe.edu.upc.center.platform.learning.infrastructure.persistence.jpa.repositories.CourseRepository;
import pe.edu.upc.center.platform.learning.infrastructure.persistence.jpa.repositories.EnrollmentRepository;
import pe.edu.upc.center.platform.learning.infrastructure.persistence.jpa.repositories.StudentRepository;

/**
 * Implementation of EnrollmentCommandService
 *
 * <p>
 *     This class is the implementation of the EnrollmentCommandService interface.
 *     It is used by the EnrollmentContext to handle commands on the Enrollment aggregate.
 * </p>
 *
 */
@Service
public class EnrollmentCommandServiceImpl implements EnrollmentCommandService {
  private final CourseRepository courseRepository;

  private final StudentRepository studentRepository;
  private final EnrollmentRepository enrollmentRepository;

  /**
   * Constructor
   *
   * @param courseRepository     CourseRepository
   * @param studentRepository    StudentRepository
   * @param enrollmentRepository EnrollmentRepository
   */
  public EnrollmentCommandServiceImpl(CourseRepository courseRepository, StudentRepository studentRepository, EnrollmentRepository enrollmentRepository) {
    this.courseRepository = courseRepository;
    this.studentRepository = studentRepository;
    this.enrollmentRepository = enrollmentRepository;
  }

  /**
   * Command handler to request enrollment
   *
   * @param command containing studentRecordId and courseId
   * @return enrollmentId
   */
  public Long handle(RequestEnrollmentCommand command) {
    studentRepository.findByStudentRecordId(command.studentRecordId()).map(student -> {
      Course course = courseRepository.findById(command.courseId()).orElseThrow(() -> new CourseNotFoundException(command.courseId()));
      Enrollment enrollment = new Enrollment(command.studentRecordId(), course);
      enrollment = enrollmentRepository.save(enrollment);
      return enrollment.getId();
    }).orElseThrow(() -> new RuntimeException("Student not found"));
    return 0L;
  }

  /**
   * Command handler to confirm enrollment
   *
   * @param command containing enrollmentId
   * @return enrollmentId
   */
  @Override
  public Long handle(ConfirmEnrollmentCommand command) {
    enrollmentRepository.findById(command.enrollmentId()).map(enrollment -> {
      enrollment.confirm();
      enrollmentRepository.save(enrollment);
      return command.enrollmentId();
    }).orElseThrow(() -> new RuntimeException("Enrollment not found"));
    return null;
  }

  /**
   * Command handler to reject enrollment
   *
   * @param command containing enrollmentId
   * @return enrollmentId
   */
  @Override
  public Long handle(RejectEnrollmentCommand command) {
    enrollmentRepository.findById(command.enrollmentId()).map(enrollment -> {
      enrollment.reject();
      enrollmentRepository.save(enrollment);
      return enrollment.getId();
    }).orElseThrow(() -> new RuntimeException("Enrollment not found"));
    return null;
  }

  /**
   * Command handler to cancel enrollment
   *
   * @param command containing enrollmentId
   * @return enrollmentId
   */
  @Override
  public Long handle(CancelEnrollmentCommand command) {
    enrollmentRepository.findById(command.enrollmentId()).map(enrollment -> {
      enrollment.cancel();
      enrollmentRepository.save(enrollment);
      return enrollment.getId();
    }).orElseThrow(() -> new RuntimeException("Enrollment not found"));
    return null;
  }

  /**
   * Command handler to complete tutorial for enrollment
   *
   * @param command containing enrollmentId and tutorialId
   * @return enrollmentId
   */
  @Override
  public Long handle(CompleteTutorialForEnrollmentCommand command) {
    enrollmentRepository.findById(command.enrollmentId()).map(enrollment -> {
      enrollment.completeTutorial(command.tutorialId());
      enrollmentRepository.save(enrollment);
      return enrollment.getId();
    }).orElseThrow(() -> new RuntimeException("Enrollment not found"));
    return null;
  }
}
```

En el package `commandservices` crear la clase `StudentCommandServiceImpl` con el siguiente contenido:
```java
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.learning.domain.model.commands.CreateStudentCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.UpdateStudentMetricsOnTutorialCompletedCommand;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.AcmeStudentRecordId;
import pe.edu.upc.center.platform.learning.domain.services.StudentCommandService;

@Service
public class StudentCommandServiceImpl implements StudentCommandService {
  @Override
  public AcmeStudentRecordId handle(CreateStudentCommand command) {
    return null;
  }

  @Override
  public AcmeStudentRecordId handle(UpdateStudentMetricsOnTutorialCompletedCommand command) {
    return null;
  }
}
```

### Package application.internal eventhandlers

En el package `eventhandlers` crear la clase `TutorialCompletedEventHandler` con el siguiente contenido:
```java
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Service;
import pe.edu.upc.center.platform.learning.domain.model.commands.UpdateStudentMetricsOnTutorialCompletedCommand;
import pe.edu.upc.center.platform.learning.domain.model.events.TutorialCompletedEvent;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetEnrollmentByIdQuery;
import pe.edu.upc.center.platform.learning.domain.services.EnrollmentQueryService;
import pe.edu.upc.center.platform.learning.domain.services.StudentCommandService;

/**
 * TutorialCompletedEventHandler
 * <p>
 *     This event handler is responsible for handling TutorialCompletedEvent.
 *     It uses EventListener from Spring Boot Context Event Bus to listen to TutorialCompletedEvent.
 * </p>
 */
@Service
public class TutorialCompletedEventHandler {
  private final StudentCommandService studentCommandService;
  private final EnrollmentQueryService enrollmentQueryService;

  public TutorialCompletedEventHandler(StudentCommandService studentCommandService, EnrollmentQueryService enrollmentQueryService) {
    this.studentCommandService = studentCommandService;
    this.enrollmentQueryService = enrollmentQueryService;
  }

  /**
   * Event handler for TutorialCompletedEvent
   * <p>
   *     This method is called when TutorialCompletedEvent is fired.
   *     It fetches the enrollment by enrollmentId and updates the student metrics.
   *     It uses the {@link EnrollmentQueryService} to fetch the enrollment.
   *     It uses the {@link StudentCommandService} to update the student metrics.
   * </p>
   * @param event TutorialCompletedEvent containing enrollmentId and tutorialId
   * @see TutorialCompletedEvent
   */
  @EventListener(TutorialCompletedEvent.class)
  public void on(TutorialCompletedEvent event) {
    // Fetch enrollment by enrollmentId
    var getEnrollmentByIdQuery = new GetEnrollmentByIdQuery(event.getEnrollmentId());
    var enrollment = enrollmentQueryService.handle(getEnrollmentByIdQuery);
    if (enrollment.isPresent()) {
      // Update student metrics on tutorial completed
      var updateStudentMetricsOnTutorialCompletedCommand = new UpdateStudentMetricsOnTutorialCompletedCommand(enrollment.get().getAcmeStudentRecordId());
      studentCommandService.handle(updateStudentMetricsOnTutorialCompletedCommand);
    }
    System.out.println("TutorialCompletedEventHandler executed");
  }
}
```
