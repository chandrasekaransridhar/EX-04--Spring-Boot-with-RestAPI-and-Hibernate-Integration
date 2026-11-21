# EXPERIMENT – 04

## Spring Boot with REST API and Hibernate Integration

### AIM

To develop a Spring Boot application to store and retrieve data from a Movies database using Object Relational Mapping (ORM) with Hibernate and expose the data through REST APIs.

### ALGORITHM

1. Create a Spring Boot project using Spring Initializr.
2. Add the following dependencies:
   - Spring Web
   - Spring Data JPA
   - H2 Database or MySQL Driver
3. Configure the database connection and JPA settings in the `application.properties` file.
4. Create a Movie entity class with the following fields:
   - id
   - title
   - genre
   - year
   - rating
5. Create a `MovieRepository` interface extending `JpaRepository`.
6. Create a `MovieController` class to handle REST API requests.
7. Implement the following CRUD operations:
   - GET `/movies/get` – Retrieve all movies.
   - GET `/movies/{id}` – Retrieve a movie by ID.
   - POST `/movies/add` – Add a new movie.
   - PUT `/movies/{id}` – Update an existing movie.
   - DELETE `/movies/{id}` – Delete a movie by ID.
8. Run the Spring Boot application.
9. Test all REST API endpoints using Postman or a browser.

### PROGRAM CODE

**1. application.properties**

```properties
spring.application.name=Exp4
server.port=8082
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://localhost:3306/movies
spring.datasource.username=root
spring.datasource.password=****
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.show-sql=true
```

**2. Movie.java**

```java
package com.sridhar.exp4.Entity;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Entity
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Movie {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;
    private String title;
    private String genre;
    private Integer year;
    private Double rating;
}
```

**3. MovieRepository.java**

```java
package com.sridhar.exp4.Repository;

import com.sridhar.exp4.Entity.Movie;
import org.springframework.data.jpa.repository.JpaRepository;

public interface MovieRepository extends JpaRepository<Movie,Integer> {

}
```

**4. MovieController.java**

```java
package com.sridhar.exp4.Controller;

import com.sridhar.exp4.Entity.Movie;
import com.sridhar.exp4.Service.MovieService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/movies")
public class MovieController {
    @Autowired
    private MovieService service;

    @GetMapping("/get")
    public List<Movie> getAllMovies(){
        return service.getAllMovies();
    }

    @PostMapping("/add")
    public String addMovie(@RequestBody Movie movie){
        service.addMovie(movie);
        return "Movie Added Successfully🎥🎥";
    }

    @GetMapping("/{id}")
    public Optional<Movie> getMovie(@PathVariable int id){
        return service.getMovie(id);
    }

    @PutMapping("/{id}")
    public String updateMovie(@PathVariable int id,@RequestBody Movie movie){
        service.updateMovie(id,movie);
        return "Movie Updated Successfully.📽️🎦🍿";
    }

    @DeleteMapping("/{id}")
    public String deleteMovie(@PathVariable int id){
        service.deleteMovie(id);
        return "Movie with ID " + id + " deleted successfully!";
    }
}
```

**5. MovieService.java**

```java
package com.sridhar.exp4.Service;

import com.sridhar.exp4.Entity.Movie;
import com.sridhar.exp4.Repository.MovieRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class MovieService {
    @Autowired
    private MovieRepository repo;

    public List<Movie> getAllMovies() {
        return repo.findAll();
    }

    public void addMovie(Movie movie) {
        repo.save(movie);
    }

    public Optional<Movie> getMovie(int id) {
        return repo.findById(id);
    }

    public void updateMovie(int id, Movie movie) {
        Movie movie1=repo.findById(id).orElseThrow();
        movie1.setTitle(movie.getTitle());
        movie1.setGenre(movie.getGenre());
        movie1.setYear(movie.getYear());
        movie1.setRating(movie.getRating());
        repo.save(movie1);
    }

    public void deleteMovie(int id) {
        repo.deleteById(id);
    }
}
```

**6. Main Application Class – MovieApplication.java**

```java
package com.sridhar.exp4;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Exp4Application {

    public static void main(String[] args) {
        SpringApplication.run(Exp4Application.class, args);
    }
}
```

### REST API ENDPOINTS

| HTTP Method | Endpoint | Operation |
|---|---|---|
| GET | `/movies/get` | Get all movies |
| GET | `/movies/{id}` | Get movie by ID |
| POST | `/movies/add` | Add a new movie |
| PUT | `/movies/{id}` | Update movie |
| DELETE | `/movies/{id}` | Delete movie |

### output

**GET movies/get:**
<img width="1917" height="1075" alt="Screenshot 2026-11-20 235203" src="https://github.com/user-attachments/assets/98a9dfbb-a63a-417b-9841-290e230a662c" />


**GET movies/{id}:**

<img width="1917" height="1078" alt="Screenshot 2026-11-20 235300" src="https://github.com/user-attachments/assets/7d107cf7-adb7-42c4-8a9e-355a99c61910" />


**POST movies/add:**

<img width="1917" height="1078" alt="Screenshot 2026-11-20 235632" src="https://github.com/user-attachments/assets/6273be94-5cb5-48eb-8e9c-b9afe9d52175" />


**PUT movies/{id}:**

<img width="1917" height="1078" alt="Screenshot 2026-11-20 235836" src="https://github.com/user-attachments/assets/d1586a45-e4e2-4148-971f-34e7fe563cb0" />


**DELETE movies/{id}:**
<img width="1917" height="1078" alt="Screenshot 2026-11-21 000024" src="https://github.com/user-attachments/assets/44e53bae-11e4-4a15-8f5d-f78cbc3bd690" />



### RESULT

Thus, the Spring Boot application was successfully created to store, retrieve, update, and delete movie details using Hibernate ORM and REST APIs.
