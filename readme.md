Welcome to app.go
=================

**app.go** is a simple web framework for use in Google AppEngine. Just copy the app folder to your working folder and import it from your main program. That's it. A web application ready to run in no time. Also, app.go comes with a powerful datastore manager to simplify your interactions with BigTable, making your code cleaner and safer.


Here is the Guestbook example from AppEngine rewritten using app.go

    package hello

    import "app"

    type Greeting struct {
        Author  string
        Content string
        Date    int64
    }

    func init() {
        app.Start()
        app.Get ( "/index" , index )
        app.Post( "/sign"  , sign  )
    }

    func index(� app.Context) {
        recs := make([]Greeting, 0, 10)
        qry  := �.DB.Query("Greeting").Order("-Date").Limit(10)
        �.DB.Select(qry,&recs)
        �.Render("index",recs)
    }

    func sign(� app.Context) {
        rec := Greeting{
            Author : �.User.Nick,
            Content: �.GetValue("content"),
            Date   : �.DB.Now(),
        }
        �.DB.New(&rec)
        �.Redirect("/")
    }


As you can see, with app.go we make it really easy to write web apps in go. We welcome your feedback for any special request or bug fix.

Enjoy!


CHANGELOG v2
------------
* implement regexp router
* create new instance of DB for every request
* use nanoseconds in db.sequence
* on init: if no templates error/notfound generate default templates.
* cache templates
