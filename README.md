Breezy for Good iOS ICC v1
============================
Setup
----------------
**For ICC:**
Add the following files to your project:

- ServiceController.h
- ServiceController.m

### Step 1 - Import ServiceController.h and GDiOS.h into the desired .h file
```objc
#import <GD/GDiOS.h>
#import "ServiceController.h"
```

### Step 2 - Add the ServiceControllerDelegate to the desired .h file
Add the following delegates into your view controller
```objc
@interface YourViewController : UIViewController <ServiceControllerDelegate>
```

### Step 3 - Allocate an instance of the ServiceController, set the delegate in the desired .m file
Initialise and set the delegate of the ServiceController
```objc
@implementation GoodClientViewController
{
    ServiceController *_serviceController;
}
```

```objc
// Set up a ServiceController
_serviceController = [[ServiceController alloc] init];
[_serviceController setDelegate:self];
```

### Step 4 - Implement the following ServiceControllerDelegate methods in the desired .m file
```objc
- (void) showAlert:(id)serviceReply
{
    if ([serviceReply isKindOfClass:[NSString class]])
    {
        // The Transfer Service returned a defined success response...
        UIAlertView* alert = [[UIAlertView alloc] initWithTitle:@"Success"
                                                        message:serviceReply
                                                       delegate:nil
                                              cancelButtonTitle:@"OK"
                                              otherButtonTitles:nil];
        [alert show];
    }
    else if ([serviceReply isKindOfClass:[NSError class]])
    {
        NSError* error = (NSError*)serviceReply;
        // The Transfer Service returned a defined error response...
        UIAlertView* alert = [[UIAlertView alloc] initWithTitle:[error domain]
                                                        message:[error localizedDescription]
                                                       delegate:nil
                                              cancelButtonTitle:@"OK"
                                              otherButtonTitles:nil];
        [alert show];
    }
    else
    {
        // The Transfer Service returned an unexpected response...
        UIAlertView* alert = [[UIAlertView alloc] initWithTitle:@"Error"
                                                        message:@"Not implemented."
                                                       delegate:nil
                                              cancelButtonTitle:@"OK"
                                              otherButtonTitles:nil];
        [alert show];
    }
}
```

```objc
- (void) showErrorAlert:(NSError*)goodError
{
    // Show alert with appropriate error message
    UIAlertView* alert = [[UIAlertView alloc] initWithTitle:@"An error occurred."
                                                    message:[goodError localizedDescription]
                                                   delegate:nil
                                          cancelButtonTitle:@"OK"
                                          otherButtonTitles:nil];
    [alert show];
}
```


Objective
----------------
1. To save your document to your app's GD Container.
2. Transfer the document from your Good Dynamics app's GD Container to Breezy for Good's to print.
**Breezy for Good supports the following filetypes: Imported File, PDF, Postscript, Encapsulated PostScript, Adobe Photoshop, Adobe Illustrator, GIF, Bitmap, Microsoft Word, Microsoft Excel, Microsoft Powerpoint, Apple Keynote, eFax Fax, PNG, TIFF, JPEG, Generic Image, XML, TXT, Generic Text, Generic Mixed Content, Generic.**

How to use
----------------
Pass the name of the file you want to print to your instance of the ServiceController using the method - printFile:file withError:error
**Make sure the file is in your GD Secure Container.**

```objc
- (IBAction)sendPressed:(UIButton *)sender
{
    NSError *err = nil;
    NSString *fileToPrint = [self fileInGDContainer];
    
    BOOL didSendRequest = [_serviceController printFile:fileToPrint withError:&err];
    
    if (NO == didSendRequest)
    {
        // The request could not be sent...
        [self showErrorAlert:err];
    }
}
```

