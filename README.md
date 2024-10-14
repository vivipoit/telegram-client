# telegram-client

This is a Python script that uses [Telethon](https://github.com/LonamiWebs/Telethon) to create a Telegram group and return an invite link.

## Run locally

Follow the steps below to run the `create_group.py` script with its static string values.

1. run `git clone git@github.com:vivipoit/telegram-client.git`
2. navigate into directory
3. run `python3 -m pip install -r requirements.txt` to install packages
4. run `source env/bin/activate` to activate Python environment
5. update the `.setEnvironmentVariables.sh.example` file with your values and remove `.example` from the filename
    - `SESSION_STRING` is generated by following Telethon's String Sessions instructions [here](https://docs.telethon.dev/en/stable/concepts/sessions.html#string-sessions)
    - `API_ID` and `API_HASH` are generated by following Telegram's API instructions [here](https://core.telegram.org/api/obtaining_api_id)
6. run `chmod +x .setEnvironmentVariables.sh` to make sure the script can run
7. run `source .setEnvironmentVariables.sh` to export the variables
8. run `python create_group.py` to see code run and display the invite link for the newly created group

## Run on AWS Lambda

The code in `aws/lambda_function.py` is a bit more sophisticated, because instead of static string values, it expects the AWS Lambda event to have a body that provides the `group_title` and `link_title`.

`aws/telethon_layer.zip` is a file that can be uploaded as the layer for the function to use.

Sample request: `curl -v -d '{"group_title":"The coolest group!","link_title":"join-the-coolest-group"}' $LAMBDA_URL -H "Content-Type: application/json"`

# Cheatsheet

## Python

`python3 -m pip freeze > requirements.txt`

## AWS

### Docs

https://docs.aws.amazon.com/lambda/latest/dg/python-package.html#python-package-create-dependencies

### Blog post

https://medium.com/brlink/how-to-create-a-python-layer-in-aws-lambda-287235215b79

#### Commands
- `mkdir aws`
- `cd aws`
- `mkdir python`
- `cd python`
- `pip3 install telethon -t .`
- `cd ..`
- `zip -r telethon_layer.zip .`