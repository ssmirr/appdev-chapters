# Tower of Babel Hints

## ERD

My ERD ended up looking like the below.

<details>
  <summary>Show ERD</summary>

  <img class="img-fluid w-100" src="https://res.cloudinary.com/firstdraft/image/fetch/f_auto,q_auto:low/https://raw.githubusercontent.com/firstdraft/appdev-chapters/master/assets/towerofbabel-erd.png" alt="" />
</details>

## Suggested steps

 1. Generate [user accounts](https://chapters.firstdraft.com/chapters/773#a-special-sort-of-table-user-accounts){:target="_blank"}.
 2. Generate [chat resources](https://chapters.firstdraft.com/chapters/773#generating-a-resource){:target="_blank"}.
 3. Force users to sign in before they can do anything else.
 4. Modify the generated chat resource code to automatically associate a chat with the signed in user.
 5. Use the `Language` seed data to create a dropdown in the new chat form.
 6. There's a helper method in actions and view templates that you can use to find out the URL of the current request: `request.original_url` (to display the share link).
 7. Whenever a visitor arrives at a chat details page:
    - Check whether we know this visitor or not (look in their browser cookies).
    - If not,
        - Create a new record in a table to keep track of them. I called this table `Speaker`.
        - Default their name to "Guest" (unless they happen to be a signed in `User`, then set their name to their name).
        - Default their language to the chat's default language (unless they happen to be a signed in `User`, then set their language to English).
        - Save them.
        - Set a cookie with their newly assigned ID number in this table.
    - If so,
        - Look them up.
 8. Make a form on the chat details page to allow speakers to modify their display name and language.
 9. Make a form on the chat details page to allow speakers to add messages to the chat.
 10. Display the list of messages associated with a chat on the details page, along with the speaker's name, in ascending order that they were `created_at`.
 11. There is a view helper method called `simple_format` that will do basic formatting of long pieces of `text`, like adding line breaks.

### The crux

Now that you have the basic CRUD of the application working, the crux of the application:

 - Create a table to store translations of each message. I called mine `Translation`, with a `message_id`, `language_id`, and `body`.
 - As you are looping through the messages for a chat to display them on the details page, for each message:
 - Look up the translation of the message in the current speaker's language.
 - If it is not `nil`
    - Display it.
 - If it is `nil`,
    - Translate it with the Google Translate API and save it.
    - Then display it.

Magical.
