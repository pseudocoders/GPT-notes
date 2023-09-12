# Uploading images

**Backend (Spring Boot):**

1. **Set up a Spring Boot Project:**
   Start by creating a Spring Boot project using your preferred IDE or Spring Initializer (https://start.spring.io/). Include the necessary dependencies such as Spring Web, Spring Data JPA, and H2 or any other database of your choice.

2. **Create an Entity:**
   Define a JPA entity to represent the image data. For example:

```java
@Entity
public class Image {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String imageName;

    @Lob
    private byte[] imageData;

    // Constructors, getters, setters
}
```

3. **Create a Repository:**
   Create a Spring Data JPA repository for the `Image` entity to interact with the database.

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface ImageRepository extends JpaRepository<Image, Long> {
}
```


4. **Set Up a Controller:**
   Create a REST controller to handle image upload and retrieval requests. Implement an endpoint to upload images.

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/api/images")
public class ImageController {

    @Autowired
    private ImageRepository imageRepository;

    @GetMapping
    public ResponseEntity<List<Image>> getAllImages() {
        List<Image> images = imageRepository.findAll();
        return new ResponseEntity<>(images, HttpStatus.OK);
    }

    @GetMapping("/{id}")
    public ResponseEntity<Image> getImageById(@PathVariable Long id) {
        Optional<Image> optionalImage = imageRepository.findById(id);
        return optionalImage.map(image -> new ResponseEntity<>(image, HttpStatus.OK))
                .orElseGet(() -> new ResponseEntity<>(HttpStatus.NOT_FOUND));
    }

    @PostMapping("/upload")
    public ResponseEntity<String> uploadImage(@RequestParam("file") MultipartFile file) {
        try {
            Image image = new Image();
            image.setImageName(file.getOriginalFilename());
            image.setImageData(file.getBytes());

            imageRepository.save(image);

            return new ResponseEntity<>("Image uploaded successfully.", HttpStatus.OK);
        } catch (IOException e) {
            return new ResponseEntity<>("Failed to upload image.", HttpStatus.BAD_REQUEST);
        }
    }

    @PutMapping("/{id}")
    public ResponseEntity<String> updateImage(@PathVariable Long id, @RequestParam("file") MultipartFile file) {
        try {
            Optional<Image> optionalImage = imageRepository.findById(id);
            if (optionalImage.isPresent()) {
                Image image = optionalImage.get();
                image.setImageName(file.getOriginalFilename());
                image.setImageData(file.getBytes());

                imageRepository.save(image);

                return new ResponseEntity<>("Image updated successfully.", HttpStatus.OK);
            } else {
                return new ResponseEntity<>("Image not found.", HttpStatus.NOT_FOUND);
            }
        } catch (IOException e) {
            return new ResponseEntity<>("Failed to update image.", HttpStatus.BAD_REQUEST);
        }
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<String> deleteImage(@PathVariable Long id) {
        Optional<Image> optionalImage = imageRepository.findById(id);
        if (optionalImage.isPresent()) {
            imageRepository.deleteById(id);
            return new ResponseEntity<>("Image deleted successfully.", HttpStatus.OK);
        } else {
            return new ResponseEntity<>("Image not found.", HttpStatus.NOT_FOUND);
        }
    }
}


```

* The ImageController defines two endpoints: one to retrieve all images (/api/images) and another to upload images (/api/images/upload).
* When uploading an image, the uploadImage method receives the MultipartFile from the Angular frontend, saves it as a byte[] in the Image entity, and then stores it in the database using the imageRepository.
* The getAllImages method retrieves all images from the database and returns them as a JSON response.


**Frontend (Angular):**

**image-upload.component.html** (HTML template):

```html
<div class="container">
  <h2>Image Upload</h2>
  <form (ngSubmit)="onSubmit()" #imageForm="ngForm">
    <div class="form-group">
      <label for="imageName">Image Name:</label>
      <input type="text" id="imageName" name="imageName" [(ngModel)]="imageName" class="form-control" required>
    </div>
    <div class="form-group">
      <label for="imageFile">Choose Image:</label>
      <input type="file" id="imageFile" name="imageFile" (change)="onFileSelected($event)" class="form-control" accept="image/*" required>
    </div>
    <div class="form-group">
      <img *ngIf="previewImage" [src]="previewImage" alt="Image Preview" class="preview-image">
    </div>
    <button type="submit" class="btn btn-primary" [disabled]="imageForm.invalid || !previewImage">Upload</button>
  </form>
</div>
```

**image-upload.component.ts** (TypeScript component class):

```typescript
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-image-upload',
  templateUrl: './image-upload.component.html',
  styleUrls: ['./image-upload.component.css']
})
export class ImageUploadComponent {

  imageName: string = '';
  selectedImage: File | null = null;
  previewImage: string | ArrayBuffer | null = null;

  constructor(private http: HttpClient) { }

  onFileSelected(event: any) {
    this.selectedImage = event.target.files[0];
    this.previewSelectedImage();
  }

  previewSelectedImage() {
    if (this.selectedImage) {
      const reader = new FileReader();
      reader.readAsDataURL(this.selectedImage);
      reader.onload = () => {
        this.previewImage = reader.result;
      };
    }
  }

  onSubmit() {
    if (!this.selectedImage || !this.imageName) {
      return;
    }

    const formData = new FormData();
    formData.append('file', this.selectedImage);
    formData.append('imageName', this.imageName);

    this.http.post<any>('http://your-api-url/api/images/upload', formData).subscribe(
      response => {
        console.log('Image uploaded successfully:', response);
        // Reset form fields
        this.imageName = '';
        this.selectedImage = null;
        this.previewImage = null;
      },
      error => {
        console.error('Error uploading image:', error);
      }
    );
  }
}
```

**image-upload.component.css** (CSS styles - adjust as needed):

```css
.container {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
}

.preview-image {
  max-width: 100%;
  height: auto;
  margin-top: 10px;
}
```

In this Angular component:

- Users can input an image name and choose an image file.
- The selected image is previewed in the `<img>` element when an image is selected.
- When the user submits the form, the `onSubmit` method sends a POST request to your Spring Boot API to upload the image along with its name.
- Make sure to replace `'http://your-api-url/api/images/upload'` with the actual URL of your Spring Boot API endpoint for image upload.

Don't forget to add the necessary imports, set up the Angular FormsModule in your AppModule, and configure the HttpClientModule if you haven't already done so in your Angular application.

This is a high-level overview of the process. You will need to write code and configure your applications in more detail. Additionally, you may want to add error handling, validation, and other features based on your project requirements.
