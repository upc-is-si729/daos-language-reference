# Learning Center Platform - Part 2

## Table of contents

## Learning Bounded Context

### Project structur for Learning Bounded Context

Crear la siguiente estructura para el Bounded Context `learning`:

```markdown
- 📁 learning
  - 📁 application.internal
    - 📁 commandservices
    - 📁 eventhandlers
    - 📁 outboundservices.acl
    - 📁 queryservices
  - 📁 domain
    - 📁 exceptions
    - 📁 model
      - 📁 aggregates
      - 📁 commands
      - 📁 entities
      - 📁 events
      - 📁 queries
      - 📁 valueobjects
    - 📁 services
  - 📁 infrastructure.persistence.jpa.repositories
  - 📁 interfaces.rest
    - 📁 resources
    - 📁 transform
```

### Package domain . model

En el package `aggregates` crear las clases:
- `Course`
- `Enrollment`
- `Student`

En el package `entities` crear las clases:
- `LearningPathItem`
- `ProgressRecordItem`

### Package domain . model . valueobjects

En el package `valueobjects` crear el enum `EnrollmentStatus` con el siguiente contenido:
```java
public enum EnrollmentStatus {
  REQUESTED,
  CONFIRMED,
  REJECTED,
  CANCELLED,
}
```

En el package `valueobjects` crear el enum `ProgressStatus` con el siguiente contenido:
```java
public enum ProgressStatus {
  NOT_STARTED,
  STARTED,
  COMPLETED,
}
```

En el package `valueobjects` crear el record `AcmeStudentRecordId` con el siguiente contenido:
```java
import jakarta.persistence.Embeddable;
import java.util.UUID;

/**
 * Value object representing the Acme student record id.
 */
@Embeddable
public record AcmeStudentRecordId(String studentRecordId) {
  public AcmeStudentRecordId() {
    this(UUID.randomUUID().toString());
  }

  public AcmeStudentRecordId {
    if (studentRecordId == null || studentRecordId.isBlank()) {
      throw new IllegalArgumentException("Acme student record profileId cannot be null or blank");
    }
  }
}
```

En el package `valueobjects` crear el record `ProfileId` con el siguiente contenido:
```java
import jakarta.persistence.Embeddable;

/**
 * Value object representing the profile id.
 */
@Embeddable
public record ProfileId(Long profileId) {

  public ProfileId {
    if (profileId < 0) {
      throw new IllegalArgumentException("Profile profileId cannot be negative");
    }
  }

  public ProfileId() {
    this(0L);
  }
}
```

En el package `valueobjects` crear el record `StudentPerformanceMetricSet` con el siguiente contenido:
```java
import jakarta.persistence.Embeddable;

/**
 * Value object representing the student performance metrics.
 */
@Embeddable
public record StudentPerformanceMetricSet(Integer totalCompletedCourses, Integer totalTutorials) {
  public StudentPerformanceMetricSet() {
    this(0, 0);
  }

  public StudentPerformanceMetricSet {
    if (totalCompletedCourses < 0) {
      throw new IllegalArgumentException("Total completed courses cannot be negative");
    }
    if (totalTutorials < 0) {
      throw new IllegalArgumentException("Total tutorials cannot be negative");
    }
  }

  public StudentPerformanceMetricSet incrementTotalCompletedCourses() {
    return new StudentPerformanceMetricSet(totalCompletedCourses + 1, totalTutorials);
  }

  public StudentPerformanceMetricSet incrementTotalTutorials() {
    return new StudentPerformanceMetricSet(totalCompletedCourses, totalTutorials + 1);
  }
}
```

En el package `valueobjects` crear el record `TutorialId` con el siguiente contenido:
```java
import jakarta.persistence.Embeddable;

/**
 * Value object representing the tutorial id.
 */
@Embeddable
public record TutorialId(Long tutorialId) {

  public TutorialId {
    if (tutorialId < 0) {
      throw new IllegalArgumentException("Tutorial tutorialId cannot be negative");
    }
  }

  public TutorialId() {
    this(0L);
  }
}
```

En el package `valueobjects` crear las clases: 
- `LearningPath`
- `ProgressRecord`

### Package domain . model . events

En el package `events` crear la clase `TutorialCompletedEvent` con el siguiente contenido:
```java
import lombok.Getter;
import org.springframework.context.ApplicationEvent;

/**
 * TutorialCompletedEvent
 * <p>
 *     This event is fired when a tutorial is completed.
 *     It contains enrollmentId and tutorialId.
 *     It is used by TutorialCompletedEventHandler to update student metrics.
 * </p>
 * Revisar TutorialCompletedEventHandler
 */
@Getter
public final class TutorialCompletedEvent extends ApplicationEvent {

  private final Long enrollmentId;

  private final Long tutorialId;

  public TutorialCompletedEvent(Object source, Long enrollmentId, Long tutorialId) {
    super(source);
    this.enrollmentId = enrollmentId;
    this.tutorialId = tutorialId;
  }
}
```

### Package domain . model . aggregates

En el package `aggregates` modificar la clase `Student` con el siguiente contenido:
```java
import jakarta.persistence.*;
import lombok.Getter;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.StudentPerformanceMetricSet;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.StudentRecordId;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.ProfileId;
import pe.edu.upc.center.platform.shared.domain.model.aggregates.AuditableAbstractAggregateRoot;

/**
 * Represents a student.
 * The student is an aggregate root.
 */
@Entity
public class Student extends AuditableAbstractAggregateRoot<Student> {

  @Getter
  @Embedded
  @Column(name = "student_id")
  private final StudentRecordId studentRecordId;

  @Embedded
  private ProfileId profileId;

  @Embedded
  private StudentPerformanceMetricSet performanceMetricSet;

  public Student() {
    this.studentRecordId = new StudentRecordId();
    this.performanceMetricSet = new StudentPerformanceMetricSet();
  }

  public Student(Long profileId) {
    this();
    this.profileId = new ProfileId(profileId);
  }

  public Student(ProfileId profileId) {
    this();
    this.profileId = profileId;
  }

  /**
   * Updates the student metrics when a course is completed.
   * It increments the total completed courses.
   *
   */
  public void updateMetricsOnCourseCompleted() {
    this.performanceMetricSet = this.performanceMetricSet.incrementTotalCompletedCourses();
  }

  /**
   * Updates the student metrics when a tutorial is completed.
   * It increments the total completed tutorials.
   *
   */
  public void updateMetricsOnTutorialCompleted() {
    this.performanceMetricSet = this.performanceMetricSet.incrementTotalTutorials();
  }

  public String getStudentId() {
    return this.studentRecordId.studentId();
  }

  public Long getProfileId() {
    return this.profileId.profileId();
  }

  public Integer getTotalCompletedCourses() {
    return this.performanceMetricSet.totalCompletedCourses();
  }

  public Integer getTotalTutorials() {
    return this.performanceMetricSet.totalTutorials();
  }
}
```

En el package `aggregates` modificar la clase `Course` con el siguiente contenido:
```java
import jakarta.persistence.*;
import lombok.Getter;
import org.apache.logging.log4j.util.Strings;
import pe.edu.upc.center.platform.learning.domain.model.commands.CreateCourseCommand;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.LearningPath;
import pe.edu.upc.center.platform.shared.domain.model.aggregates.AuditableAbstractAggregateRoot;

@Getter
@Entity
public class Course extends AuditableAbstractAggregateRoot<Course> {

  private String title;
  private String description;
  /**
   * The learning path for this course.
   */
  @Embedded
  private final LearningPath learningPath;

  public Course() {
    this.title = Strings.EMPTY;
    this.description = Strings.EMPTY;
    this.learningPath = new LearningPath();
  }

  public Course(String title, String description) {
    this();
    this.title = title;
    this.description = description;
  }

  public Course(CreateCourseCommand command) {
    this();
    this.title = command.title();
    this.description = command.description();
  }

  /**
   * Updates the course information.
   * @param title The new title.
   * @param description The new description.
   * @return The updated course.
   */
  public Course updateInformation(String title, String description) {
    this.title = title;
    this.description = description;
    return this;
  }

  /**
   * Adds a tutorial to the learning path.
   * @param tutorialId The tutorial to add.
   */
  public void addTutorialToLearningPath(Long tutorialId) {
    System.out.println("Adding tutorial to learning path");
    this.learningPath.addItem(this, tutorialId);
  }

  /**
   * Adds a tutorial to the learning path.
   * @param tutorialId The tutorial to add.
   * @param nextTutorialId The id of the tutorial before which the new item should be added
   */
  public void addTutorialToLearningPath(Long tutorialId, Long nextTutorialId) {
    this.learningPath.addItem(this, tutorialId, nextTutorialId);
  }
}
```

En el package `aggregates` modificar la clase `Enrollment` con el siguiente contenido:
```java
import jakarta.persistence.Embedded;
import jakarta.persistence.Entity;
import jakarta.persistence.JoinColumn;
import jakarta.persistence.ManyToOne;
import lombok.Getter;
import pe.edu.upc.center.platform.learning.domain.model.events.TutorialCompletedEvent;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.ProgressRecord;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.StudentRecordId;
import pe.edu.upc.center.platform.learning.domain.model.valueobjects.EnrollmentStatus;
import pe.edu.upc.center.platform.shared.domain.model.aggregates.AuditableAbstractAggregateRoot;

/**
 * Represents an enrollment.
 * The enrollment is an aggregate root.
 */
@Entity
public class Enrollment extends AuditableAbstractAggregateRoot<Enrollment> {

  @Getter
  @Embedded
  private StudentRecordId studentRecordId;

  @Getter
  @ManyToOne
  @JoinColumn(name = "course_id")
  private Course course;

  /**
   * The progress record for this enrollment.
   */
  @Embedded
  private ProgressRecord progressRecord;

  private EnrollmentStatus status;


  public Enrollment() {
  }

  public Enrollment(StudentRecordId studentRecordId, Course course) {
    this.studentRecordId = studentRecordId;
    this.course = course;
    this.status = EnrollmentStatus.REQUESTED;
    this.progressRecord = new ProgressRecord();
  }

  /**
   * Confirms the enrollment.
   */
  public void confirm() {
    this.status = EnrollmentStatus.CONFIRMED;
    this.progressRecord.initializeProgressRecord(this, course.getLearningPath());
    // this.registerEvent(new EnrollmentConfirmedEvent(this));
  }

  /**
   * Rejects the enrollment.
   */
  public void reject() {
    this.status = EnrollmentStatus.REJECTED;
    // this.registerEvent(new EnrollmentRejectedEvent(this));
  }

  /**
   * Cancels the enrollment.
   */
  public void cancel() {
    this.status = EnrollmentStatus.CANCELLED;
    // this.registerEvent(new EnrollmentCancelledEvent(this));
  }

  /**
   * Returns the status of the enrollment.
   * @return The status of the enrollment.
   */
  public String getStatus() {
    return this.status.name().toLowerCase();
  }

  /**
   * Returns how many days have elapsed since the enrollment was confirmed.
   * @return The number of days elapsed since the enrollment was confirmed.
   */
  public long calculateDaysElapsed() {
    return progressRecord.calculateDaysElapsedForEnrollment(this);
  }

  /**
   * Returns if the enrollment is confirmed.
   * @return true if the enrollment is confirmed. Otherwise, false.
   */
  public boolean isConfirmed() {
    return this.status == EnrollmentStatus.CONFIRMED;
  }

  /**
   * Returns if the enrollment is cancelled.
   * @return true if the enrollment is cancelled. Otherwise, false.
   */
  public boolean isRejected() {
    return this.status == EnrollmentStatus.REJECTED;
  }

  /**
   * Marks a tutorial as completed in progress record.
   * @param tutorialId The id of the tutorial to mark as completed.
   *                   The tutorial must be part of the learning path of the course.
   *                   Otherwise, an exception will be thrown.
   */
  public void completeTutorial(Long tutorialId) {
    progressRecord.completeTutorial(tutorialId, course.getLearningPath());
    // Publish a Tutorial Completed Event
    this.registerEvent(new TutorialCompletedEvent(this, this.getId(), tutorialId));
  }
}
```

### Package domain . model . entities

En el package `entities` modificar la clase `LearningPathItem` con el siguiente contenido:
```java
import jakarta.persistence.*;
import jakarta.validation.constraints.NotNull;
import lombok.Getter;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Course;
import pe.edu.upc.center.platform.shared.domain.model.entities.AuditableModel;

/**
 * Represents an item in the learning path.
 */
@Getter
@Entity
public class LearningPathItem extends AuditableModel {
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  @ManyToOne
  @JoinColumn(name = "course_id")
  @NotNull
  private Course course;

  @NotNull
  private Long tutorialId;

  @ManyToOne
  @JoinColumn(name = "next_item_id")
  private LearningPathItem nextItem;

  public LearningPathItem(Course course, Long tutorialId, LearningPathItem nextItem) {
    this.course = course;
    this.tutorialId = tutorialId;
    this.nextItem = nextItem;
  }

  public LearningPathItem() {
    this.tutorialId = 0L;
    this.nextItem = null;
  }

  /**
   * Updates the next item in the learning path.
   * @param nextItem The next item.
   */
  public void updateNextItem(LearningPathItem nextItem) {
    this.nextItem = nextItem;
  }
}
```

En el package `entities` modificar la clase `ProgressRecordItem` con el siguiente contenido:
```java
import jakarta.persistence.*;
import lombok.Getter;

import java.time.LocalDate;
import java.util.Date;

/**
 * Represents an item in the progress record.
 */
@Getter
@Entity
public class ProgressRecordItem extends AuditableModel {
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  @ManyToOne
  @JoinColumn(name = "enrollment_id")
  private Enrollment enrollment;

  @Getter
  private Long tutorialId;

  private ProgressStatus status;

  private Date startedAt;

  private Date completedAt;
  
  public ProgressRecordItem(Enrollment enrollment, Long tutorialId) {
    this.enrollment = enrollment;
    this.tutorialId = tutorialId;
    this.status = ProgressStatus.NOT_STARTED;
  }

  public ProgressRecordItem() {
  }

  /**
   * Starts the item.
   */
  public void start() {
    this.status = ProgressStatus.STARTED;
    this.startedAt = new Date();
  }

  /**
   * Completes the item.
   */
  public void complete() {
    this.status = ProgressStatus.COMPLETED;
    this.completedAt = new Date();
  }

  /**
   * Returns a boolean indicating if the item is completed.
   *
   * @return true if the item is completed.
   */
  public boolean isCompleted() {
    return this.status == ProgressStatus.COMPLETED;
  }

  /**
   * Returns a boolean indicating if the item is in progress.
   *
   * @return true if the item is in progress.
   */
  public boolean isInProgress() {
    return this.status == ProgressStatus.STARTED;
  }

  /**
   * Returns a boolean indicating if the item is not started.
   *
   * @return true if the item is not started.
   */
  public boolean isNotStarted() {
    return this.status == ProgressStatus.NOT_STARTED;
  }

  /**
   * Calculates the number of days elapsed since the item was started
   *
   * @return zero if not started. Otherwise, it returns the number of days elapsed between the started date and the completed date (if completed) or today
   */
  public long calculateDaysElapsed() {

    // If not started, return 0
    if (this.status == ProgressStatus.NOT_STARTED) return 0;

    var defaultTimeZone = java.time.ZoneId.systemDefault();
    // Only started items are registered, so it should be the started date
    var fromDate = this.startedAt.toInstant();
    // If completed it should be the completed date, otherwise it should be today
    var toDate = this.completedAt == null ? LocalDate.now().atStartOfDay(defaultTimeZone).toInstant() : this.completedAt.toInstant();

    return java.time.Duration.between(fromDate, toDate).toDays();
  }
}
```

### Package domain . model . valueobjects

En el package `valueobjects` modificar la clase `LearningPath` con el siguiente contenido:
```java
import jakarta.persistence.CascadeType;
import jakarta.persistence.Embeddable;
import jakarta.persistence.OneToMany;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Course;
import pe.edu.upc.center.platform.learning.domain.model.entities.LearningPathItem;

import java.util.ArrayList;
import java.util.List;

/**
 * Represents a learning path of tutorials for a course
 * It is embedded in the Course aggregate
 * It is a value object that is not persisted in the database
 * It is initialized when a course is created
 * It is updated when a tutorial is added to the course
 * It is the base reference to track the progress of a student in a course
 * It is used to determine the next tutorial to be completed by a student
 */
@Embeddable
public class LearningPath {

  @OneToMany(mappedBy = "course", cascade = CascadeType.ALL)
  private List<LearningPathItem> learningPathItems;

  public LearningPath() {
    this.learningPathItems = new ArrayList<>();
  }
  
  /**
   * Adds the item before the item with the given id
   * @param course The course to add
   * @param tutorialId The tutorial to add
   * @param nextItem The id of the item before which the new item should be added
   */
  public void addItem(Course course, Long tutorialId, LearningPathItem nextItem) {
    // Add the new item before the next item
    System.out.println("Adding item to learning path");
    LearningPathItem learningPathItem = new LearningPathItem(course, tutorialId, nextItem);
    System.out.println("tutorial Id " + learningPathItem.getTutorialId());
    learningPathItems.add(learningPathItem);
  }

  /**
   * Adds the item at the end of the learning path
   * @param course The course to add
   * @param tutorialId The tutorial to add
   */
  public void addItem(Course course, Long tutorialId) {
    System.out.println("Adding item to learning path");
    LearningPathItem learningPathItem = new LearningPathItem(course, tutorialId, null);
    LearningPathItem originalLastItem = null;
    if (!learningPathItems.isEmpty()) {
      originalLastItem = getLastItemInLearningPath();
    } else {
      System.out.println("Learning path is empty");
    }

    learningPathItems.add(learningPathItem);
    System.out.println("tutorial Id " + learningPathItem.getTutorialId());
    System.out.println("Learning path item added");
    if (originalLastItem != null) 
      originalLastItem.updateNextItem(learningPathItem);
  }

  /**
   * Adds the item at the end of the learning path
   * @param course The course to add
   * @param tutorialId The tutorial to add
   * @param nextTutorialId The id of the tutorial before which the new item should be added
   */
  public void addItem(Course course, Long tutorialId, Long nextTutorialId) {
    LearningPathItem nextItem = getLearningPathItemWithTutorialId(nextTutorialId);
    addItem(course, tutorialId, nextItem);
  }

  public Long getFirstTutorialIdInLearningPath() {
    return learningPathItems.get(0).getTutorialId();
  }

  public Long getNextTutorialInLearningPath(Long currentTutorialId) {
    LearningPathItem item = getLearningPathItemWithTutorialId(currentTutorialId);
    return item != null ? item.getTutorialId() : null;
  }

  private LearningPathItem getLearningPathItemWithId(Long itemId) {
    return learningPathItems.stream().filter(learningPathItem -> learningPathItem.getId().equals(itemId))
            .findFirst().orElse(null);
  }

  public LearningPathItem getLearningPathItemWithTutorialId(Long tutorialId) {
    return learningPathItems.stream().filter(learningPathItem -> learningPathItem.getTutorialId().equals(tutorialId))
            .findFirst().orElse(null);
  }

  public boolean isLastTutorialInLearningPath(Long currentTutorialId) {
    return getNextTutorialInLearningPath(currentTutorialId) == null;
  }

  public LearningPathItem getLastItemInLearningPath() {
    return learningPathItems.stream().filter(item -> item.getNextItem() == null)
            .findFirst().orElse(null);
  }

  public boolean isEmpty() {
    return learningPathItems.isEmpty();
  }
}
```

En el package `valueobjects` modificar la clase `ProgressRecord` con el siguiente contenido:
```java
import jakarta.persistence.CascadeType;
import jakarta.persistence.Embeddable;
import jakarta.persistence.OneToMany;
import pe.edu.upc.center.platform.learning.domain.model.aggregates.Enrollment;
import pe.edu.upc.center.platform.learning.domain.model.entities.ProgressRecordItem;

import java.util.ArrayList;
import java.util.List;

/**
 * ProgressRecord is an entity that is embedded in Enrollment aggregate.
 * It is a value object that is not persisted in the database.
 * It is used to track the progress of a student in a learning path.
 * It is initialized when an enrollment is created.
 * It is updated when a student starts or completes a tutorial.
 */
@Embeddable
public class ProgressRecord {

  @OneToMany(mappedBy = "enrollment", cascade = CascadeType.ALL)
  private List<ProgressRecordItem> progressRecordItems;

  public ProgressRecord() {
    progressRecordItems = new ArrayList<>();
  }

  public void initializeProgressRecord(Enrollment enrollment, LearningPath learningPath) {
    if (learningPath.isEmpty()) 
      return;
    Long tutorialId = learningPath.getFirstTutorialIdInLearningPath();
    ProgressRecordItem progressRecordItem = new ProgressRecordItem(enrollment, tutorialId);
    progressRecordItems.add(progressRecordItem);
  }

  public void startTutorial(Long tutorialId) {
    if (hasAnItemInProgress()) 
      throw new IllegalStateException("A tutorial is already in progress");

    ProgressRecordItem progressRecordItem = getProgressRecordItemWithTutorialId(tutorialId);
    if (progressRecordItem != null) {
      if (progressRecordItem.isNotStarted()) 
        progressRecordItem.start();
      else 
        throw new IllegalStateException("Tutorial with given Id is already started or completed");
    }
    else 
      throw new IllegalArgumentException("Tutorial with given Id not found in progress record");
  }
  public void completeTutorial(Long tutorialId, LearningPath learningPath) {
    ProgressRecordItem progressRecordItem = getProgressRecordItemWithTutorialId(tutorialId);
    if (progressRecordItem != null) 
      progressRecordItem.complete();
    else 
      throw new IllegalArgumentException("Tutorial with given Id not found in progress record");

    if (learningPath.isLastTutorialInLearningPath(tutorialId)) 
      return;

    Long nextTutorialId = learningPath.getNextTutorialInLearningPath(tutorialId);
    if (nextTutorialId != null) {
      ProgressRecordItem nextProgressRecordItem = new ProgressRecordItem(progressRecordItem.getEnrollment(), nextTutorialId);
      progressRecordItems.add(nextProgressRecordItem);
    }
  }

  private ProgressRecordItem getProgressRecordItemWithTutorialId(Long tutorialId) {
    return progressRecordItems.stream().filter(progressRecordItem -> progressRecordItem.getTutorialId().equals(tutorialId))
            .findFirst().orElse(null);
  }

  private boolean hasAnItemInProgress() {
    return progressRecordItems.stream().anyMatch(ProgressRecordItem::isInProgress);
  }

  public long calculateDaysElapsedForEnrollment(Enrollment enrollment) {
    return progressRecordItems.stream().filter(progressRecordItem -> progressRecordItem.getEnrollment().equals(enrollment))
            .mapToLong(ProgressRecordItem::calculateDaysElapsed).sum();
  }
}
```

