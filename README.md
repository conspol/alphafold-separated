Slightly modified version of [AlphaFold 2](https://github.com/deepmind/alphafold).
Allows to run CPU-intensive pre-processing pipeline to prepare `features.pkl` file separately 
from the GPU-intensive prediction part by creating different docker containers.

To install, follow the steps from the original repo up to building the docker image.
Instead of the original command for building, use:
- `docker build -f docker/Dockerfile_features -t af2_features .` to build the CPU-intensive part.
- `docker build -f docker/Dockerfile_predict -t af2_predict .` to build the GPU-intensive part.

Run the `af2_features` container first. It will create `features.pkl` files for each protein. 
Run `af2_predict` after the first feature file is done. \
Alternatively, run `af2_predict` with pre-made feature files placed in corresponding output folders.
The syntax to run the containers is the same as in the original AlphaFold 2. \
If the prediction is faster than the feature preparation and the `features.pkl` is missing, 
the `af2_features` container will create `features.pkl` file by itself. In contrast to 
the vanilla behaviour, it will create the file before the features are calculated, so `af2_features`
is not going to try to process the same features https://github.com/chshr/alphafold-separated.gitagain.