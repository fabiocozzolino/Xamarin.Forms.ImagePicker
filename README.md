# Xamarin.Forms.ImagePicker

Simple ImagePicker (camera and gallery) with cropper for Xamarin.Forms.

### Usage

Inject `IImagePickerService` using your favorite container. Service implementation register is already done by Xamarin.Forms.DependencyAttribute, I tested using Unity container and it works fine with constructor parameter injection. You may also resolve with `Xamarin.Forms.DependencyService.Get<IImagePickerService>()`.

Example:

```cs
class ViewModel : System.ComponentModel.INotifyPropertyChanged
{
  public event PropertyChangedEventHandler PropertyChanged;
  private Xamarin.Forms.ImageSource _imageSource;

  public Xamarin.Forms.ImageSource ImageSource 
  { 
    get { return _imageSource; }
    set
    {
      _imageSource = value;
      PropertyChanged?.Invoke(nameof(ImageSource));
    }
  }
  
  async void PickImage() 
  {
    // Get service (recommended to use constructor parameter aproach instead)
    IImagePickerService imagePickerService = Xamarin.Forms.DependencyService.Get<IImagePickerService>();
    
    // Pick the image
    ImageSource = await imagePickerService.PickAsync();
  }
}
```

On View:

```xml
<Image Source="{Binding ImageSource}" />
```

Get JPEG stream to save on filesystem or send over network: 

```cs
System.IO.Stream stream = await imagePickerService.ImageSourceUtility.ToJpegStreamAsync(imageSource);

// ...

// release when done
stream.Dispose();
```
