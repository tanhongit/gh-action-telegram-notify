# gh-action-telegram-notify

This repository contains a simple setup for sending Telegram notifications through GitHub Actions.
It allows you to stay informed about various events and activities related to your GitHub repository.

---

**Professional related project:** [**_lbiltech/telegram-bot-github-notify_**](https://github.com/lbiltech/telegram-bot-github-notify.git)

# Information

- This action sends a message to a Telegram chat using a bot.
- The bot must be created using [@BotFather](https://telegram.me/botfather).
- Using [appleboy/telegram-action](https://github.com/appleboy/telegram-action) as a base.

# How it works

The workflow in this repository is configured to trigger various events such as pushes,
pull requests, issue updates, releases, comments, and more.
When any of these events occur, a notification will be sent to your Telegram chat using a Telegram bot.

The notifications will provide relevant information about the event,
such as the event type, repository name, user who initiated the event,
commit SHA, and other useful details.
This can help you keep track of activities and changes happening in your repository,
allowing for better collaboration and project management.

# Setup

## 1. Create a bot using [@BotFather](https://telegram.me/botfather).

Once all the details have been entered,
you'll receive the confirmation message for your bot along with the details of the token.
Copy the token key as we would be required later on.
(It would be in the ##########:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx format,
where # are numbers and x are alphanumeric characters)

After that, open the link of your bot in the message and press start. (There should be no response) This is to get the unique chat id, which tells the telegram bot to notify only you.

Go to https://api.telegram.org/bot<YourBOTToken>/getUpdates,
and replace <YourBOTToken> with the token you copied earlier.
You should see a JSON response with a chat id. Copy the chat id.

## 2. Create Secret Variables

Go to your repository **Settings > Secrets and variables** and add the following environment variables:

- `TELEGRAM_TO`: The chat id of the user to notify.
- `TELEGRAM_TOKEN`: The token of the bot created in the previous step.

![image](https://github.com/tanhongit/gh-action-telegram-notify/assets/35853002/c81374c3-9c5e-4eab-9610-9ba9e9e584b7)

## 3. Create a workflow

Create a `.github/workflows/telegram-notify.yml` file in your GitHub repo and add the following code:

```yml
name: Telegram Notification
on:
  push:
    branches:
      - main
      - develop
  pull_request:
    branches:
      - main
      - develop
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Send message to Telegram
        uses: appleboy/telegram-action@master
        with:
          to: ${{ secrets.TELEGRAM_TO }}
          token: ${{ secrets.TELEGRAM_TOKEN }}
          message: |  #https://help.github.com/en/actions/reference/contexts-and-expression-syntax-for-github-actions#github-context
            ${{ github.event_name }} commit in ${{ github.repository }} by "${{ github.actor }}". [${{github.sha}}@${{ github.ref }}]
```

> Visit https://help.github.com/en/actions/reference/context-and-expression-syntax-for-github-actions for more available environment variables.

**_You can also check my [workflow](https://github.com/tanhongit/gh-action-telegram-notify/blob/main/.github/workflows/telegram-notify.yml) for reference._**

## 4. Push a commit to your repo

```bash
git add .
git commit -m "add telegram notification"
git push origin main
```

## 5. Check the action tab

![image](https://github.com/tanhongit/gh-action-telegram-notify/assets/35853002/b5d009ed-12b8-42bf-bc60-8f7490f37559)

## 6. Check the telegram message

![image](https://github.com/tanhongit/gh-action-telegram-notify/assets/35853002/e67222cf-5993-49b9-b0c5-ac96c9018714)

