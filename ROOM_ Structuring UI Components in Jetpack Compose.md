# Building Persistence with Room Database in Android

---

### Introduction:

The Room persistence library provides a way to access SQLite database thus it is more robust. Room Persistence Library is a powerful tool for managing local data storage in Android applications. It simplifies interactions with SQLite, the underlying database engine, offering a safer and more convenient way to store and access data within your app. This article explores the fundamentals of using Room Database in Android, including setting up, defining data models, performing CRUD operations, and integrating it with ViewModels.

### Adding the Dependency:

The first step is to include the Room library dependency in your build.gradle file:

```gradle
dependencies {
       implementation ("androidx.room:room-ktx:$ROOM_VERSION" annotationProcessor "androidx.room:room-compiler:$ROOM_VERSION")
}
```  

Replace $ROOM_VERSION with the latest version of Room available in the AndroidX libraries.

  

### Initializing the Database:

To create and interact with your database, define a Room database class annotated with @Database. This class specifies the entities (data models) that the database holds and their version.
```kotlin
@Database(
  entities = [User::class],version = 1)
abstract class AppDatabase : RoomDatabase() { abstract fun userDao(): UserDao }
```
In this example, the AppDatabase holds a single entity named User and has a version number of 1. You'll also need to define an interface for interacting with the data, which we'll cover next.

### Creating Entities:

Entities represent the data structure stored in the database. Define them as data classes annotated with @Entity. Each field within the entity maps to a column in the database table.
```kotlin
@Entity(tableName = "users")
data class User(@PrimaryKey(autoGenerate = true)
          val id: Long,
          val name: String,
          val email: String)
```
Here, the User entity has three fields: id (primary key), name, and email. The @PrimaryKey annotation marks the ID field as the primary key, ensuring unique identification for each user record.

### DAOs (Data Access Objects):

Data Access Objects (DAOs) define methods for interacting with the database, including CRUD (Create, Read, Update, Delete) operations. An interface annotated with @Dao acts as the DAO for your entities.
```kotlin
@Dao
	interface UserDao {
		@Insert(onConflict = OnConflictStrategy.REPLACE)
		suspend fun insertUser(user: User)
		
		@Query("SELECT \* FROM users")
		suspend fun getAllUsers(): List<User>
		
		@Update
		suspend fun updateUser(user: User)
		
		@Delete
		suspend fun deleteUser(user: User)
	}
```
This UserDao interface defines methods for inserting, fetching all, updating, and deleting user objects. The methods are annotated with appropriate queries (@Insert, @Query, @Update, @Delete) and use keywords like suspend to indicate asynchronous database operations.

### Implementing CRUD Operations:

The actual database access logic resides within the Room database class implementation. You extend the generated abstract class and provide implementations for the defined DAO methods.
```kotlin
@Database(entities = [User::class], version = 1)
	abstract class AppDatabase : RoomDatabase() {
		abstract fun userDao(): UserDao
		
		private val callback = object : RoomDatabase.Callback() {
			override fun onCreate(db: SupportSQLiteDatabase) {
			super.onCreate(db)
			// Optional: Pre-populate the database on creation
			}
		}
	companion object {
		private var INSTANCE: AppDatabase? = null
		
		fun getInstance(context: Context): AppDatabase {
			if (INSTANCE == null) {
				synchronized(AppDatabase::class.java) {
					INSTANCE = Room.databaseBuilder(context.applicationContext, AppDatabase::class.java, "user\_database")
						.fallbackToDestructiveMigration()
						.addCallback(callback)
						.build()
					}
				}
				return INSTANCE!!
			}
		}
	}

```
In this example, AppDatabase provides implementations for the UserDao methods. These methods delegate the actual database operations to Room using functions like insert, query, etc. 

### Using Room with ViewModels:

ViewModels are ideal for managing data lifecycle within activities or fragments. To use Room with ViewModels, inject the database instance and access DAOs within the ViewModel:
```kotlin
class UserViewModel(private val database: AppDatabase) : ViewModel() {
		suspend fun getAllUsers(): List<User> {
			return database.userDao().getAllUsers()
		}
		
		suspend fun insertUser(user: User) {
			database.userDao
		}
}
```

## Resources:

<div>
  <a href="https://developer.android.com/training/data-storage/room/">
    <img src="https://i.postimg.cc/L6Hqbpqr/android-developers.png" style="width:500px; height:300px; object-fit: cover;" 
  </a>
  <br>
  <a href="https://www.youtube.com/playlist?list=PLSrm9z4zp4mEPOfZNV9O-crOhoMa0G2-o">
    <img src="https://i.ytimg.com/vi/lwAvI3WDXBY/hqdefault.jpg?sqp=-oaymwEXCNACELwBSFryq4qpAwkIARUAAIhCGAE=&rs=AOn4CLALPdTPPtnzLjvxOOKhwg44HGf-sw" style="width:500px; height:300px; object-fit: cover;"  />
  </a>
  <br>
  <a href="https://www.youtube.com/watch?v=bOd3wO0uFr8&pp=ygUXcGhpbGlwcCBsYWNrbmVyIHJvb20gZGI%3D">
    <img src="https://img.youtube.com/vi/bOd3wO0uFr8/sddefault.jpg" style="width:500px; height:350px; object-fit: cover;"  />
 </a>
</div>

## Important Questions:

*Question:* How does ROOM contribute to the scalability and maintainability of UI code in Jetpack Compose applications?
*Answer:* ROOM is a structured UI component organisation approach, which contributes towards maintainability and scalability. ROOMs represent different parts of the user interface (UI) making it easier for developers to manage or update various portions independently. Consequently, incorporating extra features, modifying already available code and teaming up with colleagues becomes simpler than ever before.

  

*Question:* Can you explain the concept of nesting ROOMs and how it can be utilised in UI design?
*Answer:* Nesting ROOMs involves organising UI components within larger containers or sections, creating a hierarchical structure. This allows for a more granular organisation of the UI, with each nested ROOM representing a specific feature or functionality. Nesting ROOMs can be utilised to break down complex UIs into smaller, more manageable components, making it easier to understand and maintain the codebase.

  

*Question:* What are the best practices for naming and organising ROOMs to ensure clarity and consistency in code?
*Answer:* Best practices for naming and organising ROOMs include using descriptive names that reflect the purpose or functionality of each ROOM. It's important to establish a consistent naming convention to ensure clarity and readability across the codebase. Additionally, organising ROOMs based on related functionality or features can help developers quickly locate and understand different parts of the UI.

  

*Question:* How does ROOM handle complex UI layouts and interactions, such as nested scrolling and gesture-based interactions?
*Answer:* ROOM provides a flexible and intuitive way to handle complex UI layouts and interactions in Jetpack Compose applications. By nesting ROOMs and leveraging layout components like Column, Row, and Box, developers can create sophisticated UIs that support nested scrolling and gesture-based interactions. ROOM's declarative approach to UI development simplifies the implementation of complex layouts and interactions, improving the overall user experience.

  

*Question:*Can you discuss any performance considerations or potential pitfalls when using ROOM in Jetpack Compose?**
*Answer:* While ROOM offers many benefits for structuring UI code, developers should be mindful of potential performance considerations. Nesting too many ROOMs or including overly complex layouts within a single ROOM can impact app performance, leading to increased rendering times and decreased responsiveness. It's important to strike a balance between modularizing the UI and maintaining optimal performance by keeping ROOM structures simple and concise. Additionally, developers should regularly profile their apps to identify and address any performance bottlenecks related to ROOM usage.
