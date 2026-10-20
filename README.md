# Movie Picture Pipeline

[![Backend Continuous Integration](https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/actions/workflows/backend-ci.yaml/badge.svg)](https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/actions/workflows/backend-ci.yaml)
[![Frontend Continuous Integration](https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/actions/workflows/frontend-ci.yaml/badge.svg)](https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/actions/workflows/frontend-ci.yaml)
[![Backend Continuous Deployment](https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/actions/workflows/backend-cd.yaml/badge.svg)](https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/actions/workflows/backend-cd.yaml)
[![Frontend Continuous Deployment](https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/actions/workflows/frontend-cd.yaml/badge.svg)](https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/actions/workflows/frontend-cd.yaml)

My repo: https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline

## About this project

This is my work for the Movie Picture Pipeline project. The movie catalog has a React frontend and a Flask backend. I set up GitHub Actions so that both apps are checked automatically and then deployed to a Kubernetes cluster on AWS.

## My four workflows

They are all inside `.github/workflows/`.

- **Backend CI** (`backend-ci.yaml`): runs on pull requests to `main` that change the backend, or by hand. Linting and testing run side by side. The Docker build only starts if both of them pass.
- **Frontend CI** (`frontend-ci.yaml`): works the same way for the frontend. It uses npm caching in the lint, test and build jobs (the log of the latest run shows a cache hit), and the Docker build gets the backend address through `REACT_APP_MOVIE_API_URL`.
- **Backend CD** (`backend-cd.yaml`): runs when backend code is pushed to `main`, or by hand. After lint and test pass, it builds the image, tags it with the git commit SHA, pushes it to Amazon ECR and deploys it to EKS.
- **Frontend CD** (`frontend-cd.yaml`): does the same for the frontend.

My AWS keys are kept in GitHub Secrets, and the backend address for the frontend build is kept in a GitHub Actions variable. Nothing private is written in the code.

## Where the apps ran

- Frontend: http://a496da9e478f14f0da058885fea8cc4b-467672457.us-east-1.elb.amazonaws.com
- Backend: http://a36c39406a2a247a7aef5f55515aecc2-1262890636.us-east-1.elb.amazonaws.com
- Movie list from the backend: http://a36c39406a2a247a7aef5f55515aecc2-1262890636.us-east-1.elb.amazonaws.com/movies

I may switch off the AWS resources after the review to save credits, so these links might stop working later. The screenshots in the repo show them working.

## Deployment summary

What is running on the cluster right now:

| | Backend | Frontend |
|---|---|---|
| Image repository | `750960077820.dkr.ecr.us-east-1.amazonaws.com/backend` | `750960077820.dkr.ecr.us-east-1.amazonaws.com/frontend` |
| Image tag | git commit SHA (commit `4c3bd45`) | git commit SHA (commit `4c3bd45`) |
| Deployed by | Backend CD run 39210756711 | Frontend CD run 39211260319 |
| Application link | http://a36c39406a2a247a7aef5f55515aecc2-1262890636.us-east-1.elb.amazonaws.com/movies | http://a496da9e478f14f0da058885fea8cc4b-467672457.us-east-1.elb.amazonaws.com |

Every image is tagged with the full git commit SHA, so each deployment can be traced back to the exact commit that produced it.

## Proof that everything passed

Green runs of each workflow:

- Backend CI: https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/actions/runs/39210480277
- Frontend CI: https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/actions/runs/39366477430
- Backend CD: https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/actions/runs/39210756711
- Frontend CD: https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/actions/runs/39211260319

The CI workflows also ran on real pull requests: [pull request 1](https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/pull/1) for the backend and [pull request 2](https://github.com/Ragul2507/cd12354-Movie-Picture-Pipeline/pull/2) for the frontend.

## Screenshots

All screenshots are in the `Screenshots` folder of this repo:

1. `1-backend-movies.png`: the backend `/movies` response
2. `2-frontend-movie-list.png`: the frontend showing the movie list
3. `3-frontend-cd.png`: a successful frontend deployment run
4. `4-backend-cd.png`: a successful backend deployment run
5. `5-backend-ci.png`: a successful backend CI run
6. `6-frontend-ci.png`: a successful frontend CI run
