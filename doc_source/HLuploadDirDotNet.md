--------

Welcome to the new **Amazon S3 User Guide**\! The Amazon S3 User Guide combines information and instructions from the three retired guides: *Amazon S3 Developer Guide*, *Amazon S3 Console User Guide*, and *Amazon S3 Getting Started Guide*\.

--------

# Uploading a directory using the high\-level \.NET `TransferUtility` class<a name="HLuploadDirDotNet"></a>

You can use the `TransferUtility` class to upload an entire directory\. By default, the API uploads only the files at the root of the specified directory\. You can, however, specify recursively uploading files in all of the subdirectories\. 

To select files in the specified directory based on filtering criteria, specify filtering expressions\. For example, to upload only the \.pdf files from a directory, specify the `"*.pdf"` filter expression\. 

When uploading files from a directory, you don't specify the key names for the resulting objects\. Amazon S3 constructs the key names using the original file path\. For example, assume that you have a directory called `c:\myfolder` with the following structure:

**Example**  

```
1. C:\myfolder
2.       \a.txt
3.       \b.pdf
4.       \media\               
5.              An.mp3
```

When you upload this directory, Amazon S3 uses the following key names:

**Example**  

```
1. a.txt
2. b.pdf
3. media/An.mp3
```

**Example**  
The following C\# example uploads a directory to an Amazon S3 bucket\. It shows how to use various `TransferUtility.UploadDirectory` overloads to upload the directory\. Each successive call to upload replaces the previous upload\. For instructions on how to create and test a working sample, see [Running the Amazon S3 \.NET Code Examples](UsingTheMPDotNetAPI.md#TestingDotNetApiSamples)\.  

```
using Amazon;
using Amazon.S3;
using Amazon.S3.Transfer;
using System;
using System.IO;
using System.Threading.Tasks;

namespace Amazon.DocSamples.S3
{
    class UploadDirMPUHighLevelAPITest
    {
        private const string existingBucketName = "*** bucket name ***";
        private const string directoryPath = @"*** directory path ***";
        // The example uploads only .txt files.
        private const string wildCard = "*.txt";
        // Specify your bucket region (an example region is shown).
        private static readonly RegionEndpoint bucketRegion = RegionEndpoint.USWest2;
        private static IAmazonS3 s3Client;
        static void Main()
        {
            s3Client = new AmazonS3Client(bucketRegion);
            UploadDirAsync().Wait();
        }

        private static async Task UploadDirAsync()
        {
            try
            {
                var directoryTransferUtility =
                    new TransferUtility(s3Client);

                // 1. Upload a directory.
                await directoryTransferUtility.UploadDirectoryAsync(directoryPath,
                    existingBucketName);
                Console.WriteLine("Upload statement 1 completed");

                // 2. Upload only the .txt files from a directory 
                //    and search recursively. 
                await directoryTransferUtility.UploadDirectoryAsync(
                                               directoryPath,
                                               existingBucketName,
                                               wildCard,
                                               SearchOption.AllDirectories);
                Console.WriteLine("Upload statement 2 completed");

                // 3. The same as Step 2 and some optional configuration. 
                //    Search recursively for .txt files to upload.
                var request = new TransferUtilityUploadDirectoryRequest
                {
                    BucketName = existingBucketName,
                    Directory = directoryPath,
                    SearchOption = SearchOption.AllDirectories,
                    SearchPattern = wildCard
                };

                await directoryTransferUtility.UploadDirectoryAsync(request);
                Console.WriteLine("Upload statement 3 completed");
            }
            catch (AmazonS3Exception e)
            {
                Console.WriteLine(
                        "Error encountered ***. Message:'{0}' when writing an object", e.Message);
            }
            catch (Exception e)
            {
                Console.WriteLine(
                    "Unknown encountered on server. Message:'{0}' when writing an object", e.Message);
            }
        }
    }
}
```