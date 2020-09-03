# Room Persistence Library 

Room provides an abstraction layer over SQLite to allow fluent database access while harnessing the full power of SQLite.

#### Advantage of Room Persistence library 

1. Compile time validation of Raw Sql Quries.
2. Room maps our database objects to Java Object without boilerplate code.
3. Room is built to work with LiveData and RxJava for data observation.

<p align="center">
  <img  src="1_nPLp8XsB7e529f82XgddyA.png">
</p>


#### Room has three main components of Room DB :

1. Entity
2. Dao (Data Access Object)
3. Database


##### Entity 

Represents a table within the database.

Entities annotations

1. @Entity —>  Every model class with this annotation will have a mapping table in DB.

2. @PrimaryKey —>  As its name indicates, this annotation points the primary key of the entity. 
   autoGenerate — if set to true, then SQLite will be generating a unique id for the column
   
3. @ColumnInfo — allows specifying custom information about column   

4. @Ignore — Field will not be persisted by Room

5. @Embeded — Nested fields can be referenced directly in the SQL queries.


##### Dao

DAOs are responsible for defining the methods that access the database.

##### Database 

It is the main class that’s annotated with @Database should satisfy the following conditions:

Be an abstract class that extends RoomDatabase.

1. Include the list of entities associated with the database within the annotation.

2. Contain an abstract method that has 0 arguments and returns the class that is annotated with @Dao.

3. At runtime, you can acquire an instance of Database by calling Room.databaseBuilder() or Room.inMemoryDatabaseBuilder().


##### Step 1 : Add dependencies 


```sh
dependencies {
  def room_version = "2.2.5"

  implementation "androidx.room:room-runtime:$room_version"
  kapt "androidx.room:room-compiler:$room_version"

  // optional - Kotlin Extensions and Coroutines support for Room
  implementation "androidx.room:room-ktx:$room_version"

  // optional - Test helpers
  testImplementation "androidx.room:room-testing:$room_version"
}

```

##### Step 2 : Create the Model class that is our Entity 

```java
@Entity
data class Student(
    @PrimaryKey(autoGenerate = true) @ColumnInfo(name = "id") val id: Int,
    @ColumnInfo(name = "firstName") var firstName: String,
    @ColumnInfo(name = "lastName") var lastName: String,
    @ColumnInfo(name = "dob") var dob: Date,
    @ColumnInfo(name = "address") var address: String
)
```

##### Step 3 : Create the DAO 

```java

@Dao
interface DaoStudent {

    @Query("SELECT * FROM Student")
    fun loadAll(): List<Student>

    @Delete
    fun deleteStudent(student: Student)

    @Query("SELECT * from Student where firstName==:name")
    fun getStudentListByName(name: String): List<Student>

    @Insert
    fun insertStudent(student: Student)

}


```

