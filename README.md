# Authorizing Requests

## Learning Goals

- Understand the difference between _authentication_ and _authorization_.
- Restrict access to routes to authorized users only.

***

## Key Vocab

- **Identity and Access Management (IAM)**: a subfield of software engineering that
  focuses on users, their attributes, their login information, and the resources
  that they are allowed to access.
- **Authentication**: proving one's identity to an application in order to
  access protected information; logging in.
- **Authorization**: allowing or disallowing access to resources based on a
  user's attributes.
- **Session**: the time between a user logging in and logging out of a web
  application.
- **Cookie**: data from a web application that is stored by the browser. The
  application can retrieve this data during subsequent sessions.

***

## Introduction

So far, we've been talking about how to **authenticate** users, i.e., how to
confirm that a user is who they say they are. We've been using their username as
our means of authentication; in the future, we'll also add a password to our
authentication process.

In addition to **authentication**, most applications also need to implement
**authorization**: giving certain users permission to access specific resources.
For example, we might want **all** users to be able to browse blog posts, but
only **authenticated** users to have access to premium features, like creating
their own blog posts. In this lesson, we'll learn how we can use the session
object to authenticate users' requests, and give them explicit permission to
access certain routes in our application.

***

## First Pass: Manual Checks

Let's say we have a `DocumentsController`. Its `show` method looks like this:

```ruby
def show
  document = Document.find(params[:id])
  render json: document
end
```

Now let's add a new requirement: documents should only be shown to users when
they're logged in. From a technical perspective, what does it actually mean for
a user to _log in_? When a user logs in, all we are doing is using cookies to
add their `:user_id` to the `session` hash.

The first thing you might do is to add a **guard clause** as the first line of
`DocumentsController#show`:

```ruby
def show
  return render json: { error: "Not authorized" }, status: :unauthorized unless session.include? :user_id
  document = Document.find(params[:id])
  render json: document
end
```

Unless the session includes `:user_id`, we return an error. `status:
:unauthorized` will return the specified HTTP status code. In this case, if a
user isn't logged in, we return `401 Unauthorized`.

***

## Resources

- [Introduction to Identity and Access Management (IAM) - auth0](https://auth0.com/docs/get-started/identity-fundamentals/identity-and-access-management)
- [Flask-Login](https://flask-login.readthedocs.io/en/latest/)
