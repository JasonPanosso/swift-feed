# Swift Feed

This repository is solely used to manage the continuous deployment (CD)
workflow for the Swift Feed frontend and backend repositories. Using GitHub
Actions, any push to the main branch in this repository (typically for updating
the commit hash references in the submodules) triggers the rebuilding of Docker
images for the frontend and backend. The Dockerfiles used for building the
images are sourced from their respective frontend and backend repositories.
Afterward, an SSH connection is established with the VPS acting as the manager
node for the Docker Swarm, and the stack is updated with the new images fetched
from Docker Hub.

While this setup has some(many) drawbacks, it currently functions well, and
optimizing CD is not a priority at the moment.

There are a few required GitHub Actions secrets in order for deployment to work:

- DOCKERHUB_USERNAME
- DOCKERHUB_TOKEN
- VPS_HOST
- VPS_PORT
- VPS_PRIVATE_KEY
- VPS_USERNAME
- BACKEND_API_URL (I hate that this exists, very hacky workaround that is used
  to get the backend api url into a .env for vite when the frontend is built)

Included in this repository are examples for backend.env and nginx.conf. If you
intend to use HTTPS/FTPS, please note that the Docker Compose file expects the
SSL certificates to be located in the ./certs directory. Ideally, the use of
Docker secrets should handle this requirement(as well as backend.env), but
dealing with certificate expiration & updating secrets with new certificates was
annoying(see: me dumb, will fix).

Please feel free to adjust and optimize this setup according to your needs and
preferences.
