# Embedded Write

Coming soon...

No, I've, here's an interesting question. If we fetch a single `CheeseListing`, you
can see the `username` that comes through on the owner property. And obviously if we,
edited a specific cheese listing, we can totally change the owner to new owner. 
Let's actually try this. Let's set just set owner two `/api/users/2` cool. 
Is that saves here's, can we get back the link to that user?

And that's great. Could we also, but looking back on the item in point, could we also
just change the `username` like so instead of changing the actual owner, she'll can we
edit the owners username while we're updating a `CheeseListing`? It's kind of a weird
example in this case, but let's try it. So instead of owner being an IRI, let's say
that we want to say, well, let's set it to an object.

And if we actually add a user, set it to an object and set the username of, try it,
it doesn't work. It says Nesta documents for attribute owner are not allowed. Use
IRIs instead. So it looks like this isn't possible. But that's not actually true.

Yeah.

When we are the whole reason that using an of shows up here as we've edited towards
`cheese_listing:item:get` Group, which is the group that is used when we are using that
are get itemOperation. We also could have used `cheese_listing:read`. If you
want this to be writeable daring denormalization, then we just need to use the 
denormalization group, which in this case is `cheese_listing:write`? So I'm
gonna copy that and actually add that over here. Suddenly that actually does make
this embedded username property modifiable. So if we go back and try it and now, so
we're still have owner with username and it execute.

Yeah.

Oh, it still doesn't work. But the air is fascinating. A new entity was found through
the relationship cheese listing owner that was not configured to cascade persist
operations per entity. If you've been around doctrine for awhile, this means that
behind the scenes API Platform actually created a new `User` object and it failed
because nothing ever persisted. That this is something we can work around. Um, and
we'll talk about cascading persist operations in the second, but this is actually not
what we want. What we wanted is we wanted it to take the existing user an update.
It's username instead of creating a totally new username. And the way to do that, the
problem here is actually just my syntax. The way the API Platform works is that if
you pass it just an array of data, it assumes that's a new object and tries to create
a new object. If you want to signal that there's an existing object, you need to add
the `@id` property. So `/api/users/2` it's going to load that user trying to modify
and try to modify it. So if you try that this time it works and we can't see what the
actual username is here. But if we go down and look for um, username `id = 2`, there it
is our culture cheese at.

So does that mean that we could actually create a brand new user

while editing a cheese listing? Like, could we put, you know, could we take off as ad
id and I had username and password and the answer is yes. In theory this is maybe not
something that you even want to do. What you need to do is make sure that you expose
all of the properties that you need. So for example, our and our `User` entity, we
actually would also need to expose the pro, the `$password` field and the `$email` field
because these are both required things of the database. So right now we couldn't
actually create a new user even if we cascade things correctly because we haven't
exposed enough fields. But we aren't going to talk a little bit more about that. Uh,
and the second, when we start treating in a second, but one thing that is weird here,
if you did want to do this kind of stuff is that if you set owner `username` to empty
quotes, that shouldn't work because we have an `@NotBlank()` for `$username` which makes 
perfect sense. But if you try it you get,

Oh of course. In 500 error cause I took off my `@id`. Let me put that back 
`@id: "/api/users/2`. There we go. Let's try that again. Gas. Here we go. You getting
200 status code. It appears to have worked. And in fact if you go down here and look
up user out with `id = 2`, they have no username. This is a bit of a Gotcha. It's how
the about it deals with validation by defaults. When we modify the cheese listing,
the validation rules are executed. So are `@Assert/NotBlank()`, uh, as
`@Asseer/Length()`, these things are validated. But when the validators system sees in
embedded season object, it doesn't follow that. It doesn't actually go into the
owner, into the user and make sure it's also valid. You can't force that by saying 
`@Assert/Valid()`. Now if we go back and try our edit endpoint on execute, Yup. You can
see owner.username:. This value should not be blank. So that fixes it. So let me
change that back here.

Yeah.

To a valid user name just so we can fix that. And perfect. That one works fine. So,
you know, it's really cool that you can make modifications on embedded properties,
like on embedded objects like this that may or may not be something you actually
want. It makes things more complicated. I don't love doing it, but it is something
that you can absolutely do.