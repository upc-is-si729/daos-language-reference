# Learning Center Platform - Part 5

## Table of contents

## Package shared

### Package infrastructure documentation.openapi.configuration

En el package `configuration` crear la clase `OpenApiConfiguration` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Course;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.CourseResource;

public class CourseResourceFromEntityAssembler {

  public static CourseResource toResourceFromEntity(Course entity) {
    return new CourseResource(entity.getId(), entity.getTitle(), entity.getDescription());
  }
}
```

## Learning Bounded Context

### Package interfaces.rest resources

En el package `resources` crear el record `CourseResource` con el siguiente contenido:
```java
public record CourseResource(Long id, String title, String description) {
}
```

En el package `resources` crear el record `StudentResource` con el siguiente contenido:
```java
public record StudentResource(String studentId, Long profileId, Integer totalCompletedCourses,
                              Integer totalTutorials) {
}
```

En el package `resources` crear el record `EnrollmentResource` con el siguiente contenido:
```java
public record EnrollmentResource(Long enrollmentId, String studentId, Long courseId,
                                 String status) {
}
```

En el package `resources` crear el record `CreateCourseResource` con el siguiente contenido:
```java
public record CreateCourseResource(String title, String description) {
}
```

En el package `resources` crear el record `CreateStudentResource` con el siguiente contenido:
```java
public record CreateStudentResource(String firstName, String lastName, String email, 
                                    String street, String number, String city, String postalCode, String country) {
}
```

En el package `resources` crear el record `LearningPathItemResource` con el siguiente contenido:
```java
public record LearningPathItemResource(Long learningPathItemId, Long courseId, Long tutorialId) {
}
```

En el package `resources` crear el record `RequestEnrollmentResource` con el siguiente contenido:
```java
import jakarta.validation.constraints.NotNull;

public record RequestEnrollmentResource(
        @NotNull
        String studentId,
        @NotNull
        Long courseId
) {
}
```

En el package `resources` crear el record `UpdateCourseResource` con el siguiente contenido:
```java
public record UpdateCourseResource(String title, String description) {
}
```

### Package interfaces.rest transform

En el package `transform` crear la clase `CourseResourceFromEntityAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Course;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.CourseResource;

public class CourseResourceFromEntityAssembler {

  public static CourseResource toResourceFromEntity(Course entity) {
    return new CourseResource(entity.getId(), entity.getTitle(), entity.getDescription());
  }
}
```

En el package `transform` crear la clase `StudentResourceFromEntityAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Student;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.StudentResource;

public class StudentResourceFromEntityAssembler {

  public static StudentResource toResourceFromEntity(Student student) {
    return new StudentResource(
            student.getStudentId(),
            student.getProfileId(),
            student.getTotalCompletedCourses(),
            student.getTotalTutorials()
    );
  }
}
```

En el package `transform` crear la clase `EnrollmentResourceFromEntityAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Enrollment;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.EnrollmentResource;

/**
 * EnrollmentResourceFromEntityAssembler.
 * <p>
 * This class is used to transform an Enrollment entity into an EnrollmentResource.
 * </p>
 */
public class EnrollmentResourceFromEntityAssembler {

  /**
   * Transform an Enrollment entity into an EnrollmentResource.
   *
   * @param entity Enrollment entity to be transformed.
   * @return EnrollmentResource the resulting resource.
   */
  public static EnrollmentResource toResourceFromEntity(Enrollment entity) {
    return new EnrollmentResource(entity.getId(), entity.getStudentRecordId().studentId(), entity.getCourse().getId(), entity.getStatus());
  }
}
```

En el package `transform` crear la clase `CreateCourseCommandFromResourceAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.commands.CreateCourseCommand;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.CreateCourseResource;

public class CreateCourseCommandFromResourceAssembler {

  public static CreateCourseCommand toCommandFromResource(CreateCourseResource resource) {
    return new CreateCourseCommand(resource.title(), resource.description());
  }
}
```

En el package `transform` crear la clase `CreateStudentCommandFromResourceAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.commands.CreateStudentCommand;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.CreateStudentResource;

public class CreateStudentCommandFromResourceAssembler {

  public static CreateStudentCommand toCommandFromResource(CreateStudentResource resource) {
    return new CreateStudentCommand(
            resource.firstName(),
            resource.lastName(),
            resource.email(),
            resource.street(),
            resource.number(),
            resource.city(),
            resource.postalCode(),
            resource.country()
    );
  }
}
```

En el package `transform` crear la clase `LearningPathItemResourceFromEntityAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.entities.LearningPathItem;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.LearningPathItemResource;

public class LearningPathItemResourceFromEntityAssembler {

  public static LearningPathItemResource toResourceFromEntity(LearningPathItem entity) {
    return new LearningPathItemResource(entity.getId(), entity.getCourse().getId(), entity.getTutorialId());
  }
}
```

En el package `transform` crear la clase `RequestEnrollmentCommandFromResourceAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.commands.RequestEnrollmentCommand;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.StudentRecordId;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.RequestEnrollmentResource;

public class RequestEnrollmentCommandFromResourceAssembler {

  public static RequestEnrollmentCommand toCommandFromResource(RequestEnrollmentResource resource) {
    return new RequestEnrollmentCommand(new StudentRecordId(resource.studentId()), resource.courseId());
  }
}
```

En el package `transform` crear la clase `UpdateCourseCommandFromResourceAssembler` con el siguiente contenido:
```java
import pe.edu.upc.center.platform.learning.domain.model.commands.UpdateCourseCommand;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.UpdateCourseResource;

public class UpdateCourseCommandFromResourceAssembler {

  public static UpdateCourseCommand toCommandFromResource(Long courseId, UpdateCourseResource resource) {
    return new UpdateCourseCommand(courseId, resource.title(), resource.description());
  }
}
```

### Package interfaces.rest

En el package `rest` crear la clase `CoursesController` con el siguiente contenido:
```java
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import pe.edu.upc.center.platform.learning.domain.model.commands.DeleteCourseCommand;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetAllCoursesQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetCourseByIdQuery;
import pe.edu.upc.center.platform.learning.domain.services.CourseCommandService;
import pe.edu.upc.center.platform.learning.domain.services.CourseQueryService;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.CourseResource;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.CreateCourseResource;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.UpdateCourseResource;
import pe.edu.upc.center.platform.learning.interfaces.rest.transform.CourseResourceFromEntityAssembler;
import pe.edu.upc.center.platform.learning.interfaces.rest.transform.CreateCourseCommandFromResourceAssembler;
import pe.edu.upc.center.platform.learning.interfaces.rest.transform.UpdateCourseCommandFromResourceAssembler;

import java.util.List;

import static org.springframework.http.MediaType.APPLICATION_JSON_VALUE;

/**
 * Courses Controller.
 * <p>
 * This class is the entry point for all the REST API calls related to courses.
 * It is responsible for handling the requests and delegating the processing to the appropriate services.
 * It also transforms the data from the request to the appropriate commands and vice versa.
 * <ul>
 *     <li>POST /api/v1/courses</li>
 *     <li>GET /api/v1/courses/{courseId}</li>
 *     <li>GET /api/v1/courses</li>
 *     <li>PUT /api/v1/courses/{courseId}</li>
 *     <li>DELETE /api/v1/courses/{courseId}</li>
 * </ul>
 * </p>
 *
 *
 */
@RestController
@RequestMapping(value = "/api/v1/courses", produces = APPLICATION_JSON_VALUE)
@Tag(name = "Courses", description = "Course Management Endpoints")
public class CoursesController {
  private final CourseCommandService courseCommandService;
  private final CourseQueryService courseQueryService;

  public CoursesController(CourseCommandService courseCommandService, CourseQueryService courseQueryService) {
    this.courseCommandService = courseCommandService;
    this.courseQueryService = courseQueryService;
  }

  /**
   * Creates a new course.
   *
   * @param createCourseResource the resource containing the data for the course to be created
   * @return the created course resource
   * @see CreateCourseResource
   * @see CourseResource
   */
  @PostMapping
  public ResponseEntity<CourseResource> createCourse(@RequestBody CreateCourseResource createCourseResource) {
    var createCourseCommand = CreateCourseCommandFromResourceAssembler.toCommandFromResource(createCourseResource);
    var courseId = courseCommandService.handle(createCourseCommand);

    if (courseId == 0L) {
      return ResponseEntity.badRequest().build();
    }
    var getCourseByIdQuery = new GetCourseByIdQuery(courseId);
    var course = courseQueryService.handle(getCourseByIdQuery);

    if (course.isEmpty())
      return ResponseEntity.badRequest().build();
    var courseResource = CourseResourceFromEntityAssembler.toResourceFromEntity(course.get());
    return new ResponseEntity<>(courseResource, HttpStatus.CREATED);
  }

  /**
   * Gets a course by its id.
   *
   * @param courseId the id of the course to be retrieved
   * @return the course resource with the given id
   * @see CourseResource
   */
  @GetMapping("/{courseId}")
  public ResponseEntity<CourseResource> getCourseById(@PathVariable Long courseId) {
    var getCourseByIdQuery = new GetCourseByIdQuery(courseId);
    var course = courseQueryService.handle(getCourseByIdQuery);

    if (course.isEmpty())
      return ResponseEntity.badRequest().build();
    var courseResource = CourseResourceFromEntityAssembler.toResourceFromEntity(course.get());
    return ResponseEntity.ok(courseResource);
  }

  /**
   * Gets all the courses.
   *
   * @return the list of all the course resources
   * @see CourseResource
   */
  @GetMapping
  public ResponseEntity<List<CourseResource>> getAllCourses() {
    var getAllCoursesQuery = new GetAllCoursesQuery();
    var courses = courseQueryService.handle(getAllCoursesQuery);
    var courseResources = courses.stream().map(CourseResourceFromEntityAssembler::toResourceFromEntity).toList();
    return ResponseEntity.ok(courseResources);
  }

  /**
   * Updates a course.
   *
   * @param courseId             the id of the course to be updated
   * @param updateCourseResource the resource containing the data for the course to be updated
   * @return the updated course resource
   * @see UpdateCourseResource
   * @see CourseResource
   */
  @PutMapping("/{courseId}")
  public ResponseEntity<CourseResource> updateCourse(@PathVariable Long courseId, @RequestBody UpdateCourseResource updateCourseResource) {
    var updateCourseCommand = UpdateCourseCommandFromResourceAssembler.toCommandFromResource(courseId, updateCourseResource);
    var updatedCourse = courseCommandService.handle(updateCourseCommand);

    if (updatedCourse.isEmpty()) {
      return ResponseEntity.badRequest().build();
    }
    var courseResource = CourseResourceFromEntityAssembler.toResourceFromEntity(updatedCourse.get());
    return ResponseEntity.ok(courseResource);
  }

  /**
   * Deletes a course.
   *
   * @param courseId the id of the course to be deleted
   * @return Deletion confirmation message
   */
  @DeleteMapping("/{courseId}")
  public ResponseEntity<?> deleteCourse(@PathVariable Long courseId) {
    var deleteCourseCommand = new DeleteCourseCommand(courseId);
    courseCommandService.handle(deleteCourseCommand);
    return ResponseEntity.ok("Course with given id successfully deleted");
  }
}
```

En el package `rest` crear la clase `StudentsController` con el siguiente contenido:
```java
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetStudentByAcmeStudentRecordIdQuery;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.AcmeStudentRecordId;
import pe.edu.upc.center.platform.learning.domain.services.StudentCommandService;
import pe.edu.upc.center.platform.learning.domain.services.StudentQueryService;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.CreateStudentResource;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.StudentResource;
import pe.edu.upc.center.platform.learning.interfaces.rest.transform.CreateStudentCommandFromResourceAssembler;
import pe.edu.upc.center.platform.learning.interfaces.rest.transform.StudentResourceFromEntityAssembler;

/**
 * StudentsController
 *
 * <p>Controller that handles the endpoints for students.
 * It uses the {@link StudentCommandService} and {@link StudentQueryService} to handle the commands and queries
 * for students.
 * <ul>
 *     <li>POST /api/v1/students</li>
 *     <li>GET /api/v1/students/{studentRecordId}</li>
 * </ul>
 * </p>
 */
@RestController
@RequestMapping(value = "/api/v1/students", produces = MediaType.APPLICATION_JSON_VALUE)
@Tag(name = "Students", description = "Student Management Endpoints")
public class StudentsController {
  private final StudentCommandService studentCommandService;
  private final StudentQueryService studentQueryService;

  public StudentsController(StudentCommandService studentCommandService, StudentQueryService studentQueryService) {
    this.studentCommandService = studentCommandService;
    this.studentQueryService = studentQueryService;
  }

  /**
   * POST /api/v1/students
   *
   * <p>Endpoint that creates a student</p>
   *
   * @param resource the resource with the information to create the student
   * @return the created student
   * @see CreateStudentResource
   * @see StudentResource
   */
  @PostMapping
  public ResponseEntity<StudentResource> createStudent(@RequestBody CreateStudentResource resource) {
    var createStudentCommand = CreateStudentCommandFromResourceAssembler.toCommandFromResource(resource);
    var studentId = studentCommandService.handle(createStudentCommand);

    if (studentId.studentRecordId().isEmpty()) {
      return ResponseEntity.badRequest().build();
    }
    var getStudentByAcmeStudentRecordIdQuery = new GetStudentByAcmeStudentRecordIdQuery(studentId);
    var student = studentQueryService.handle(getStudentByAcmeStudentRecordIdQuery);

    if (student.isEmpty()) {
      return ResponseEntity.badRequest().build();
    }
    var studentResource = StudentResourceFromEntityAssembler.toResourceFromEntity(student.get());
    return new ResponseEntity<>(studentResource, HttpStatus.CREATED);

  }

  /**
   * GET /api/v1/students/{studentRecordId}
   *
   * <p>Endpoint that gets a student by its acme student record id</p>
   *
   * @param studentRecordId the acme student record id
   * @return the student resource
   * @see StudentResource
   */
  @GetMapping("/{studentRecordId}")
  public ResponseEntity<StudentResource> getStudentByAcmeStudentRecordId(@PathVariable String studentRecordId) {
    var acmeStudentRecordId = new AcmeStudentRecordId(studentRecordId);
    var getStudentByAcmeStudentRecordIdQuery = new GetStudentByAcmeStudentRecordIdQuery(acmeStudentRecordId);
    var student = studentQueryService.handle(getStudentByAcmeStudentRecordIdQuery);

    if (student.isEmpty()) {
      return ResponseEntity.notFound().build();
    }
    var studentResource = StudentResourceFromEntityAssembler.toResourceFromEntity(student.get());
    return ResponseEntity.ok(studentResource);
  }
}
```

En el package `rest` crear la clase `EnrollmentsController` con el siguiente contenido:
```java
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import pe.edu.upc.center.platform.learning.domain.model.commands.CancelEnrollmentCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.ConfirmEnrollmentCommand;
import pe.edu.upc.center.platform.learning.domain.model.commands.RejectEnrollmentCommand;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetAllEnrollmentsQuery;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetEnrollmentByAcmeStudentRecordIdAndCourseIdQuery;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.AcmeStudentRecordId;
import pe.edu.upc.center.platform.learning.domain.services.EnrollmentCommandService;
import pe.edu.upc.center.platform.learning.domain.services.EnrollmentQueryService;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.EnrollmentResource;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.RequestEnrollmentResource;
import pe.edu.upc.center.platform.learning.interfaces.rest.transform.EnrollmentResourceFromEntityAssembler;
import pe.edu.upc.center.platform.learning.interfaces.rest.transform.RequestEnrollmentCommandFromResourceAssembler;
import pe.edu.upc.center.platform.shared.interfaces.rest.resources.MessageResource;

import java.util.List;

/**
 * Inbound service for the Enrollment aggregate.
 * <p>
 * This controller is responsible for handling requests related to the Enrollment aggregate.
 * It uses the {@link EnrollmentCommandService} and {@link EnrollmentQueryService} to handle the commands and queries
 * for enrollments.
 * <ul>
 *     <li>POST /api/v1/enrollments</li>
 *     <li>POST /api/v1/enrollments/{enrollmentId}/confirmations</li>
 *     <li>POST /api/v1/enrollments/{enrollmentId}/rejections</li>
 *     <li>POST /api/v1/enrollments/{enrollmentId}/cancellations</li>
 * </ul>
 * <p>
 */
@RestController
@RequestMapping(value = "/api/v1/enrollments", produces = MediaType.APPLICATION_JSON_VALUE)
@Tag(name = "Enrollments", description = "Enrollment Management Endpoints")
public class EnrollmentsController {
  private final EnrollmentCommandService enrollmentCommandService;
  private final EnrollmentQueryService enrollmentQueryService;

  public EnrollmentsController(EnrollmentCommandService enrollmentCommandService, EnrollmentQueryService enrollmentQueryService) {
    this.enrollmentCommandService = enrollmentCommandService;
    this.enrollmentQueryService = enrollmentQueryService;
  }

  /**
   * Handles a request to enroll a student in a course.
   *
   * @param resource The request body containing the student record ID and the course ID.
   * @return The enrollment resource.
   * @see RequestEnrollmentResource
   * @see EnrollmentResource
   */
  @PostMapping
  public ResponseEntity<EnrollmentResource> requestEnrollment(@RequestBody RequestEnrollmentResource resource) {
    var command = RequestEnrollmentCommandFromResourceAssembler.toCommandFromResource(resource);
    var enrollmentId = enrollmentCommandService.handle(command);
    System.out.println("Enrollment ID: " + enrollmentId);
    var getEnrollmentByAcmeStudentRecordIdAndCourseIdQuery = new GetEnrollmentByAcmeStudentRecordIdAndCourseIdQuery(new AcmeStudentRecordId(resource.studentRecordId()), resource.courseId());
    var enrollment = enrollmentQueryService.handle(getEnrollmentByAcmeStudentRecordIdAndCourseIdQuery);

    if (enrollment.isEmpty()) {
      return ResponseEntity.notFound().build();
    }
    var enrollmentResource = EnrollmentResourceFromEntityAssembler.toResourceFromEntity(enrollment.get());
    return new ResponseEntity<>(enrollmentResource, HttpStatus.CREATED);
  }

  /**
   * Handles a request to confirm an enrollment.
   *
   * @param enrollmentId The enrollment ID.
   * @return MessageResource with The enrollment ID.
   * @see MessageResource
   */
  @PostMapping("/{enrollmentId}/confirmations")
  public ResponseEntity<MessageResource> confirmEnrollment(@PathVariable Long enrollmentId) {
    var confirmEnrollmentCommand = new ConfirmEnrollmentCommand(enrollmentId);
    enrollmentCommandService.handle(confirmEnrollmentCommand);
    return ResponseEntity.ok(new MessageResource("Confirmed Enrollment ID: " + enrollmentId));
  }

  /**
   * Handles a request to reject an enrollment.
   *
   * @param enrollmentId The enrollment ID.
   * @return MessageResource with the enrollment ID.
   * @see MessageResource
   */
  @PostMapping("/{enrollmentId}/rejections")
  public ResponseEntity<MessageResource> rejectEnrollment(@PathVariable Long enrollmentId) {
    var rejectEnrollmentCommand = new RejectEnrollmentCommand(enrollmentId);
    enrollmentCommandService.handle(rejectEnrollmentCommand);
    return ResponseEntity.ok(new MessageResource("Rejected Enrollment ID: " + enrollmentId));
  }

  /**
   * Handles a request to cancel an enrollment.
   *
   * @param enrollmentId The enrollment ID.
   * @return MessageResource with the enrollment ID.
   * @see MessageResource
   *
   */
  @PostMapping("/{enrollmentId}/cancellations")
  public ResponseEntity<MessageResource> cancelEnrollment(@PathVariable Long enrollmentId) {
    var cancelEnrollmentCommand = new CancelEnrollmentCommand(enrollmentId);
    enrollmentCommandService.handle(cancelEnrollmentCommand);
    return ResponseEntity.ok(new MessageResource("Cancelled Enrollment ID: "+ enrollmentId));
  }

  /**
   * Gets all the enrollments.
   *
   * @return The list of all the enrollment resources available.
   * @see EnrollmentResource
   */
  @GetMapping
  public ResponseEntity<List<EnrollmentResource>> getAllEnrollments() {
    var getAllEnrollmentsQuery = new GetAllEnrollmentsQuery();
    var enrollments = enrollmentQueryService.handle(getAllEnrollmentsQuery);
    var enrollmentResources = enrollments.stream().map(EnrollmentResourceFromEntityAssembler::toResourceFromEntity).toList();
    return ResponseEntity.ok(enrollmentResources);
  }
}
```

En el package `rest` crear la clase `CourseEnrollmentsController` con el siguiente contenido:
```java
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import pe.edu.upc.center.platform.learning.domain.model.queries.GetAllEnrollmentsByCourseIdQuery;
import pe.edu.upc.center.platform.learning.domain.services.EnrollmentQueryService;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.EnrollmentResource;
import pe.edu.upc.center.platform.learning.interfaces.rest.transform.EnrollmentResourceFromEntityAssembler;

import java.util.List;

import static org.springframework.http.MediaType.APPLICATION_JSON_VALUE;

/**
 * CourseEnrollmentsController
 *
 * <p>Controller that handles the endpoints for course enrollments.
 * It uses the {@link EnrollmentQueryService} to handle the queries
 * for enrollments.
 * <ul>
 *     <li>GET /api/v1/course/{courseId}/enrollments</li>
 * </ul>
 * </p>
 */
@RestController
@RequestMapping(value = "/api/v1/courses/{courseId}/enrollments", produces = APPLICATION_JSON_VALUE)
@Tag(name = "Courses")
public class CourseEnrollmentsController {
  private final EnrollmentQueryService enrollmentQueryService;

  public CourseEnrollmentsController(EnrollmentQueryService enrollmentQueryService) {
    this.enrollmentQueryService = enrollmentQueryService;
  }

  /**
   * GET /api/v1/course/{courseId}/enrollments
   *
   * <p>Endpoint that returns the enrollments for a course</p>
   *
   * @param courseId the course ID
   * @return the enrollment resources for the course with given ID
   * @see EnrollmentResource
   */
  @GetMapping
  public ResponseEntity<List<EnrollmentResource>> getAllEnrollmentsByCourseId(@PathVariable Long courseId) {
    var getAllEnrollmentsByCourseIdQuery = new GetAllEnrollmentsByCourseIdQuery(courseId);
    var enrollments = enrollmentQueryService.handle(getAllEnrollmentsByCourseIdQuery);
    var enrollmentResources = enrollments.stream().map(EnrollmentResourceFromEntityAssembler::toResourceFromEntity).toList();
    return ResponseEntity.ok(enrollmentResources);
  }
}
```

En el package `rest` crear la clase `CourseLearningPathController` con el siguiente contenido:
```java
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import pe.edu.upc.center.platform.learning.domain.model.commands.AddTutorialToCourseLearningPathCommand;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetLearningPathItemByCourseIdAndTutorialIdQuery;
import pe.edu.upc.center.platform.learning.domain.services.CourseCommandService;
import pe.edu.upc.center.platform.learning.domain.services.CourseQueryService;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.LearningPathItemResource;
import pe.edu.upc.center.platform.learning.interfaces.rest.transform.LearningPathItemResourceFromEntityAssembler;

import static org.springframework.http.MediaType.APPLICATION_JSON_VALUE;

/**
 * REST controller for managing the learning path of a course.
 * <p>
 *     This controller exposes the following endpoints:
 *     <ul>
 *         <li>POST /courses/{courseId}/learning-path/{tutorialId}: Adds a tutorial to the learning path of a course.</li>
 *     </ul>
 * </p>
 */
@RestController
@RequestMapping(value = "/courses/{courseId}/learning-path-items", produces = APPLICATION_JSON_VALUE)
@Tag(name = "Courses")
public class CourseLearningPathController {

  private final CourseCommandService courseCommandService;
  private final CourseQueryService courseQueryService;

  public CourseLearningPathController(CourseCommandService courseCommandService, CourseQueryService courseQueryService) {
    this.courseCommandService = courseCommandService;
    this.courseQueryService = courseQueryService;
  }

  /**
   * Adds a tutorial to the learning path of a course.
   * @param courseId The course identifier.
   * @param tutorialId The tutorial identifier.
   * @return The learning path item.
   * @see LearningPathItemResource
   */
  @PostMapping("{tutorialId}")
  public ResponseEntity<LearningPathItemResource> addTutorialToCourseLearningPath(@PathVariable Long courseId, @PathVariable Long tutorialId) {
    courseCommandService.handle(new AddTutorialToCourseLearningPathCommand(tutorialId, courseId));
    var getLearningPathItemByCourseIdAndTutorialIdQuery = new GetLearningPathItemByCourseIdAndTutorialIdQuery(courseId, tutorialId);
    var learningPathItem = courseQueryService.handle(getLearningPathItemByCourseIdAndTutorialIdQuery);

    if (learningPathItem.isEmpty())
      return ResponseEntity.notFound().build();
    else {
      var learningPathItemResource = LearningPathItemResourceFromEntityAssembler.toResourceFromEntity(learningPathItem.get());
      return ResponseEntity.ok(learningPathItemResource);
    }
  }
}
```

En el package `rest` crear la clase `StudentEnrollmentsController` con el siguiente contenido:
```java
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import pe.edu.upc.center.platform.learning.domain.model.queries.GetAllEnrollmentsByAcmeStudentRecordIdQuery;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.AcmeStudentRecordId;
import pe.edu.upc.center.platform.learning.domain.services.EnrollmentQueryService;
import pe.edu.upc.center.platform.learning.interfaces.rest.resources.EnrollmentResource;
import pe.edu.upc.center.platform.learning.interfaces.rest.transform.EnrollmentResourceFromEntityAssembler;

import java.util.List;

import static org.springframework.http.MediaType.APPLICATION_JSON_VALUE;

/**
 * StudentsController
 *
 * <p>Controller that handles the endpoints for students.
 * It uses the {@link EnrollmentQueryService} to handle the queries
 * for enrollments.
 * <ul>
 *     <li>GET /api/v1/students/{studentRecordId}/enrollments</li>
 * </ul>
 * </p>
 */
@RestController
@RequestMapping(value = "/api/v1/students/{studentRecordId}/enrollments", produces = APPLICATION_JSON_VALUE)
@Tag(name = "Students")
public class StudentEnrollmentsController {
  private final EnrollmentQueryService enrollmentQueryService;


  public StudentEnrollmentsController(EnrollmentQueryService enrollmentQueryService) {
    this.enrollmentQueryService = enrollmentQueryService;
  }

  /**
   * GET /api/v1/students/{studentRecordId}/enrollments
   *
   * <p>Endpoint that returns the enrollments for a student</p>
   *
   * @param studentRecordId the student record ID
   * @return the enrollments for the student
   * @see EnrollmentResource
   */
  @GetMapping
  public ResponseEntity<List<EnrollmentResource>> getEnrollmentsForStudentWithStudentRecordId(@PathVariable String studentRecordId) {
    var acmeStudentRecordId = new AcmeStudentRecordId(studentRecordId);
    var getAllEnrollmentsByAcmeStudentRecordIdQuery = new GetAllEnrollmentsByAcmeStudentRecordIdQuery(acmeStudentRecordId);
    var enrollments = enrollmentQueryService.handle(getAllEnrollmentsByAcmeStudentRecordIdQuery);
    var enrollmentResources = enrollments.stream().map(EnrollmentResourceFromEntityAssembler::toResourceFromEntity).toList();
    return ResponseEntity.ok(enrollmentResources);
  }
}
```

