# Sender Verification

Various KYC statuses can be simulated in Sandbox by supplying specific values in the First Name field. You can use the values provided in the following table to simulate the corresponding KYC status.

| First Name      | Customer Verification Status | Description                                                                                                                                   |
| --------------- | ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Retry           | Retry Case                   | The customer would be taken to the review screen whereby they will need to make the necessary amendments and submit their request once again. |
| Verified        | Verified Customer Record     | The customer status would be verified.                                                                                                        |
| Suspended       | Suspended Case               | The customer status would be suspended. In production, the client will need to contact Machnet for the suspended case.                        |
| Any other value | Random Cases                 | The KYC status will be random.                                                                                                                |

**Note:**

* If 4 digit SSN is asked, use the above details and 3333 for SSN
* If 9 digit SSN is asked, use the above details and 112223333 for SSN

## Test value for Document Verification <a href="#test-value-for-document-verification" id="test-value-for-document-verification"></a>

Provide the following guidelines to customers to assist them with taking the highest quality image possible: 1. Capture document images with at least a 5 MP camera. 2. Place the document on a dark background that contrasts with their document. 3. Ensure that the dark background fills about 10% of the top, bottom, left, and right sides of the image. 4. Confirm that all four corners of the document are clearly visible in the image. 5. The image should be in sharp focus, not rotated or skewed, and taken in a well-lit room on a camera without flash enabled. 6. The image needs to be less than 5MB in Size and should be in either .jpg or .jpeg format.

## Image for Document Verification <a href="#image-for-document-verification" id="image-for-document-verification"></a>

You can upload the following images for document verification. Please download the folder below and use the documents in the folder.

{% file src="../../../../.gitbook/assets/Inyo_Sender ID_Test values.zip" %}
