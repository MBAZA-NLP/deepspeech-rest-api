# DeepSpeech REST API

[![made-with-python](https://img.shields.io/badge/Made%20with-Python-1f425f.svg)](https://www.python.org/)
[![Open Source Love svg1](https://badges.frapsoft.com/os/v1/open-source.svg?v=103)](https://github.com/ellerbrock/open-source-badges/)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity)

---

This REST API is built on top of Mozilla's [DeepSpeech](https://github.com/mozilla/DeepSpeech). It is written based on
examples provided by Mozilla and they can be found [here](https://github.com/mozilla/DeepSpeech-examples)

It accepts HTTP methods such as GET and POST and WebSocket. To perform transcription process using HTTP methods is
appropriate for relatively short audio while websockets can be used even for long audio recordings.

_We haven't yet tested the limitations of both methods in terms of audio size._

----

## Setting up

Clone the repository on your local machine and change directory to _deepspeech-rest-api_

```shell
git clone https://github.com/fabricekwizera/deepspeech-rest-api.git && cd deepspeech-rest-api 
```

Create a virtual environment and activate it (assuming that it is installed your machine)
and install all the needed requirements.

```shell
 virtualenv -p python3 venv && source venv/bin/activate && pip install -r requirements.txt
```

----

## Download the model and the scorer

For English model and scorer, follow below links

```shell
wget https://github.com/mozilla/DeepSpeech/releases/download/v0.9.1/deepspeech-0.9.1-models.pbmm -O deepspeech_model.pbmm
wget https://github.com/mozilla/DeepSpeech/releases/download/v0.9.1/deepspeech-0.9.1-models.scorer -O deepspeech_model.scorer
```

For other languages, you can place them in the current working directory under the names _deepspeech_model.pbmm_ for the
model and _deepspeech_model.scorer_ for the scorer.

## Usage

----

### Running the server

```shell
python run.py
```

or

### Docker

Build image and run container

```shell
docker-compose up
```

- Sending speech-to-text requests to server using HTTP

```shell
curl -X POST -F "speech=@2830-3980-0043.wav" http://0.0.0.0:8000/api/v1/stt/http
```

and using the websocket (open another Shell window). It is simply running below Python script to because websockets
don't support _curl_

```shell
python test_websocket.py
```

in both cases, after transcription the response looks like

```shell
{"message": "experience proves this", "time": 1.4718825020026998}
```

- Add a hot-word

```shell
curl -X POST -d '{"football": 1.56}' http://0.0.0.0:8000/api/v1/hw/add
```

Output

```shell
{"message":" 'football' hot-word with boost '1.56' was added."}
```

- Erase a hot-word

```shell
curl -X DELETE -d '{"football": ""}' http://0.0.0.0:8000/api/v1/hw/delete
```

Output

```shell
{"message":" 'football' hot-word was erased."}
```

- Erase all hot-words

```shell
curl -X DELETE http://0.0.0.0:8000/api/v1/hw/delete/all
```

Output

```shell
{"message":"All hot-words were erased."}
```