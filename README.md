# TEEHR Workshop at the May 2023 CIROH Developers Conference
This repo contains the workshop materials that will be used for the TEEHR Workshop at the May 2023 CIROH Developers Conference.
It is assumed that the workshop participants will be working on the CIROH 2i2c JupyterHub account where the cached data is stored.


If you are working though these workshop materials on your own computer, you will need to download the cached data from our S3 bucket and change the `CACH_DIR` variable in each notebook to run the examples.

## Working Locally
If you are not working in the AWI's 2i2c JupyterHub, these instructions should get you going.  Note, they are not super well tested and could require some tweaking depending on your setup, but should provide a decent starting point.

### Using Pangeo Docker Image
```bash
# create a working directory and change into it
mkdir ~/teehr-workshop
cd teehr-workshop

# clone the workshop repo
git clone https://github.com/RTIInternational/teehr-may-2023-workshop.git
cd teehr-may-2023-workshop
git lfs install # see https://git-lfs.com/
git lfs pull
cd ..

# create a data directory
mkdir data
cd data

# fetch the test datasets (note they are 4.5 GB and uncompressed are closer to 10 GB)
wget https://ciroh-rti-public-data.s3.us-east-2.amazonaws.com/teehr-workshop-may-2023.tar.gz
tar -xzvf teehr-workshop-may-2023.tar.gz
cd ..

# launch docker and mount your home directory
docker run -it --rm --volume $HOME:$HOME -p 8888:8888 pangeo/pangeo-notebook:latest jupyter lab --ip 0.0.0.0 $HOME
```

Once you have JupyterHub running, you should be able to go to the URL that the JupyterHub provides in the terminal after starting to access the JupyterHub.  Since we mounted your home directory in the Docker container, you should be able to browse to the `teehr-workshop` working directory we created.  You should be able to open and work though the workbooks, starting with `00_outline.ipynb`, with minimal modifications.  One thing you will definitely need to do is change the `CACHE_DIR` value in each notebook.

Anywhere you find `CACHE_DIR = Path(Path.home(), "shared", "teehr-workshop")` you will need to change it to `CACHE_DIR = Path(Path.home(), "teehr-workshop", "data")` (assuming you followed the suggested naming above.  Similarly, anywhere you see `/home/jovyan/shared/teehr-workshop/` you will need to replace it with `/home/your-user-name/teehr-workshop/data/`.

It should also be possible to create a local Python virtual environment (e.g., with Conda) and install the required packages that way too, but we did not test of document that approach.
