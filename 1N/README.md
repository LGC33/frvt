
# FRVT and IREX 1:N validation package

The purpose of this validation package is to
1) validate that your software adheres to the [FRVT & IREX 1:N API](https://pages.nist.gov/frvt/api/FRVT_IREX_ongoing_1N_api.pdf),
2) ensure that NIST's execution of your library submission produces the expected output, and
3) prepare your submission package to send to NIST.

The code provided here is meant only for validation purposes and does not reflect how NIST will perform actual testing.  Please note that this validation package must be installed and run on Ubuntu 20.04.3, which can be downloaded from https://nigos.nist.gov/evaluations/ubuntu-20.04.3-live-server-amd64.iso.

# Important Notes
Please confirm that your software can handle very very large enrollment database sizes.  Specifically, please ensure that your software (especially during finalization and search) can handle processing an EDB (enrollment database) that exceeds 2<sup>32</sup> bytes in size.  In other words, use 64-bit types when you need them.

# Validation Dataset
The ../common/images directory will contain all of the images necessary for validation. Full documentation on the validation images is available at [https://pages.nist.gov/frvt/api/NIST.TN.2058.pdf](https://pages.nist.gov/frvt/api/NIST.TN.2058.pdf).

NOTE: The validation images are used for the sole purpose of validation and stress-testing your software.  The images are not necessarily representative of actual test data that will be used to evaluate the implementations.  Please do not contact NIST about actual testing with such validation-type imagery.

# Null Implementation
There is a null implementation of the FRVT & IREX 1:N API in ./src/nullImpl.  While the null implementation doesn't actually provide any real functionality, more importantly, it demonstrates mechanically how one could go about implementing, compiling, and building a library against the API.

To compile and build the null implementation from the top level validation directory, run
````console
$ ./scripts/build_null_impl.sh
````
This will place the implementation library into ./lib.

# Validation and Submission Preparation
To successfully complete the validation process, and to prepare your submission package to send to NIST, please perform the following steps:

0) If you are submitting an iris recognition or multimodal implementation,
   email irex@nist.gov to obtain the password to decrypt and extract the 
   validation dataset in ../common/images/iris.

1) Put all required configuration files into ./config.

2) Put your core implementation library and ALL dependent libraries into ./lib.

3) Put a version.txt file into ./doc, which provides version control information for the submission.

4) From the root validation directory, execute the validation script that corresponds
   to your submission.  
````
$ ./run_validate_1N.sh
````
The validation script will
* Install required packages that don't already exist on your system.  You will need sudo rights and connection to the Internet for this.
* Compile and link your library against the validation test harness.
* Run the test harness that was built against your library on the validation dataset.
* Prepare your submission archive.

5) Upon successful validation, an archive will be generated and named libfrvt_1N_\<company\>_\<three-digit submission sequence\>.v\<validation_package_version\>.tar.gz

   This archive must be properly encrypted and signed before transmission to NIST.  This must be done using the LATEST FRVT 1:N Ongoing public key linked from -
   https://www.nist.gov/itl/iad/image-group/products-and-services/encrypting-softwaredata-transmission-nist.

   For example:
````
$ gpg --default-key <ParticipantEmail> --output <filename>.gpg --encrypt \\
--recipient frvt@nist.gov --sign \\
libfrvt_1N_<company>_<three-digit submission sequence>.v<validation_package_version>.tar.gz
````

6) Submit the archive to NIST following the instructions at
   [http://pages.nist.gov/frvt/html/frvt_submission_form.html](http://pages.nist.gov/frvt/html/frvt_submission_form.html).

7) Participants must [subscribe](mailto:frvt-news+subscribe@list.nist.gov) to the FRVT mailing list to receive emails when new reports are published or announcements are made.

Send any questions or concerns regarding this validation package to frvt@nist.gov.

# Acceptance
NIST will validate the correct operation of the submissions on our platform by attempting to duplicate the submitted results using our own test driver linked with the participant's libraries.  In addition, NIST will perform a separate timing test to ensure the implementation meets the timing requirements for certain tasks as detailed in the [FRVT & IREX 1:N API document](https://pages.nist.gov/frvt/html/frvt1N.html).

Any discrepancies will be reported to the participant, and reasonable attempts will be made to resolve the issue.
