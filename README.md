## CakePHP Image Upload and Resize Component

Simple component that accepts user specified params, an uploaded image and resizes the image
via ImageMagick. If ImageMagick is not available, the component uses the GD library.

This component has been tested in CakePHP 1.2.X, 1.3.x and PHP 5

### Installation
Place the component in your app/controllers/components/ directory.

### Example 

```php
    class PhotosController extends AppController {
        var $name = 'Photos';
        var $uses = array('Photo');
        var $helpers = array('Html', 'Form');	
        var $components = array('ImageResize');

        function add() {
            if (!empty($this->data)) {

                $this->Photo->set($this->data);
                $img = $this->data['Photo']['img_file'];	// In view: <?php echo $form->file('Photo.img_file'); ?>

                if ($this->ImageResize->isUploadedFile($img)) {
                    // set the resize data for the FileUpload Component
                    // resize_data holds the resize settings
                    $prefs = array('resize_data' => array(RESIZE_WIDTH, '450', null));
                    $errors = $this->ImageResize->uploadFile($img, $prefs);
                    if (empty($errors)) {
                        if ($this->Photo->save($this->data)) {
                            $this->Session->setflash('You have successfully added the Photo.');
                            $this->redirect('/photos/');
                        } else {
                            $this->Session->setflash('An error occurred while uploading the Photo.');
                            $this->redirect('/photos/add');
                        } 
                    } else {
                        foreach ($errors as $key => $value) {
                            $this->Session->setflash('The following image resize error occured.<br />Error: ' . $value);
                            $this->redirect('/photos/add');
                        }
                    }
                } else {
                    $this->Photo->validates();
                }
            } 
        }

    }
```

## Maintained By

* Ken Seal (https://github.com/hunzinker)

## License

MIT License.
