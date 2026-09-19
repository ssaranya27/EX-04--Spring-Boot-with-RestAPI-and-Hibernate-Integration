# Exp-04-Spring-Boot-with-REST-API-and-Hibernate-Integration

## AIM:
To develop a Spring Boot application to store and retrieve data from a Movies database using Object Relational Mapping (ORM) with Hibernate and expose it via REST APIs.

## ALGORITHM:
Create Spring Boot project with dependencies:

Spring Web

Spring Data JPA

H2 or MySQL Database

Configure application.properties with DB connection and JPA settings.

Create Movie entity with fields like id, title, genre, rating, and year.

Create MovieRepository interface extending JpaRepository.

Create MovieController to define REST endpoints for CRUD operations:

GET /movies

GET /movies/{id}

POST /movies

PUT /movies/{id}

DELETE /movies/{id}


## PROGRAM CODE (Main Files):
### application.properties
```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```
### Movie.java
```
@Entity
public class Movie {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String genre;
    private int year;
    private double rating;

    // Getters and Setters
}
### MovieRepository.java
java
Copy
Edit
public interface MovieRepository extends JpaRepository<Movie, Long> {}
### MovieController.java
@RestController
@RequestMapping("/movies")
public class MovieController {
    @Autowired
    private MovieRepository repo;

    @GetMapping
    public List<Movie> getAllMovies() {
        return repo.findAll();
    }

    @GetMapping("/{id}")
    public ResponseEntity<Movie> getMovieById(@PathVariable Long id) {
        return repo.findById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }

    @PostMapping
    public Movie addMovie(@RequestBody Movie movie) {
        return repo.save(movie);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Movie> updateMovie(@PathVariable Long id, @RequestBody Movie movieDetails) {
        return repo.findById(id).map(movie -> {
            movie.setTitle(movieDetails.getTitle());
            movie.setGenre(movieDetails.getGenre());
            movie.setYear(movieDetails.getYear());
            movie.setRating(movieDetails.getRating());
            return ResponseEntity.ok(repo.save(movie));
        }).orElse(ResponseEntity.notFound().build());
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteMovie(@PathVariable Long id) {
        return repo.findById(id).map(movie -> {
            repo.delete(movie);
            return ResponseEntity.ok().build();
        }).orElse(ResponseEntity.notFound().build());
    }
}
```

## Output:

<img width="1920" height="1080" alt="Screenshot (831)" src="https://github.com/user-attachments/assets/dba55d00-cde2-45b2-9dd7-53af7a8ae3fa" />

<img width="1920" height="1080" alt="Screenshot (834)" src="https://github.com/user-attachments/assets/5718fba0-11bf-44db-a0e8-f61ffd92b153" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3d06bbc1-1d14-440c-b3e7-5479986617ea" />

