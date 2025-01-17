# BSE GitHub Actions Workflow

If you've done any work with gas-phase quantum chemistry software, it's possible you are already familiar with a project called the [Basis Set Exchange](https://www.basissetexchange.org/) (BSE).
This is a website that hosts basis set definitions for a large collection of different basis sets, which can be queried in numerous different formats that are compatible with various quantum chemistry codes.
In addition to providing an online graphical user interface for querying basis set information, it also offers a simple API that allows users to download basis set information from the command line.

In this exercise, you will create a GitHub Actions workflow in which you use a Docker image to download files from the BSE.

1. Create a Dockerfile for an image that can be used to download files from the internet.
The image should use `ubuntu:22.04` as the base image, and will need to include `wget`, `curl`, `Python`, or some other tool that can be used to download files.
The entrypoint script for this image should accept three arguments: (1) the name of the basis set (e.g., "`sto-3g`", "`6-31++g`", etc.), (2) the name of the format (e.g., "`json`", "`nwchem`", etc.), and (3) the specific atoms for which you would like to download basis set information (e.g., "`1,6-8`", "`C,8,p-17`", etc.).
For example, executing `docker run --rm <image_name> sto-3g json H-Ne` should download the STO-3G basis set for the atoms Hydrogen through Neon in a JSON format.
When writing your Dockerfile, follow best practices for Docker image creation.
Information about the BSE API is available [here](https://molssi-bse.github.io/basis_set_exchange-apidoc/reference.html).
Where the BSE documentation says "`BASE_URL`" you should use `https://www.basissetexchange.org/`.
The URL you will be using to download files from will be
`https://www.basissetexchange.org/api/basis/<basis>/format/<format>/?elements=<elements>`,
where `<basis>`, `<format>`, and `<elements>` correspond to the first, second, and (optional) third arguments to the entrypoint script.
Hint: in a bash script, the first argument to the script can be obtained as "`$1`", the second as "`$2`", etc.
2. Upload your Dockerfile to this GitHub repository, and implement a GitHub Actions workflow that executes whenever a push or PR is made to any branch.
The workflow should include a job in which you build and execute the image from Part 1.
Use a GitHub Actions matrix to run variations of the job for two different basis sets, two different file formats, and two different selections of elements.
A different variant of the job should run for each possible combination of basis, format, and element in your matrix (for a total of eight variations).
Run the Docker container in such a way as to ensure that the BSE file is accessible on the host file system.
Finally, print the contents of the downloaded file from the **host**.
4. Create another job in the same workflow, which should only run if the workflow is manually executed through a dispatch event.
The dispatch event should include three separate inputs: one for the basis set, one for the file format, and one for the elements selection.
The job should run your Docker container to download the corresponding basis set information from the BSE, and then print this information from the **host**.
