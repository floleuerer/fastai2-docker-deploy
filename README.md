# Deploy fastai2 models with Docker

A Docker image for deploying [fastai2](https://www.fast.ai/) models. Currently supports image classification models but i will add more use-cases later. The code is based on [fastai-serving](https://github.com/developmentseed/fastai-serving).

## Prerequesites

To build and test the image locally you need to install [`Docker`](https://docs.docker.com/get-docker/).

## Build

First train and export a fastai2 `Learner` with [`learn.export()`](https://docs.fast.ai/basic_train.html#Deploying-your-model). Put the model file to `app/model.pkl`.

Then build the docker image with the following command.

```
# PORT=8080 && docker build -f Dockerfile --build-arg -t org/image:tag .
```

## Run

```
docker run --rm -p 8080:8080 -t org/image:tag .
```

## Use

The API currently has two endpoints:

### `POST /analyze:predict`

Accepts a JSON request in the form:

```js
{
  "images": ['base64 encoded image',
              'another base64 encoded image]',
              '...'],
  "tta": False
}
```
Where `images` is a list of base64 encoded images and `tta` a bool variable to enable / disable [`test time augmentation`](https://docs.fast.ai/basic_train.html#Test-time-augmentation).

Returns a JSON response in the form:

```js
{
  "predictions": [
    { 
      "label": "predicted label",
      "probability": 0.873
    },
        { 
      "label": "predicted label2",
      "probability": 0.2123
    }
  ]
}

```

### `GET /analyze`

Returns an HTTP Status of `200` as long as the API is running (health check).

## client.py

See `client/client.py` for an example how to make API calls.