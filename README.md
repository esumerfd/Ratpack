Ratpack
=======

A micro web framework for Groovy
--------------------------------

Ratpack is inspired by the excellent [Sinatra][] framework for Ruby, and aims to make Groovy web development more classy.

  [Sinatra]: http://www.sinatrarb.com/


Getting Started
---------------

Ratpack is still *very* beta. But, you can start using it right now. Here's a basic "Hello, World" app:

    import ratpack.Ratpack
    import ratpack.RatpackServlet

    def app = Ratpack.app {

        get("/") {
            "Hello, World!"
        }
    }

    RatpackServlet.serve(app)

If you save the above code in `hello.groovy` and run it on the command line, it will start your app in Jetty on port 5000.


POST and Other Verbs
--------------------

    post("/submit") {
        // handle form submission here
    }

    put("/some-resource") {
        // create the resource
    }

    delete("/some-resource") {
        // delete the resource
    }

    register("propfind", "/some-resource") {
        // you can register your own verbs
    }

    register(["get", "post"], "/formpage") {
        // you can register multiple verbs to the same handler
    }


URL Parameters
--------------

You can capture parts of the URL to use in your handler code using the colon character.

    get("/person/:personid") { urlparams ->
        "This is the page for person ${urlparams.personid}"
    }

    get("/company/:companyname/invoice/:invoiceid") { inputs ->
        def company = CompanyDAO.getByName(inputs.companyname)
        def invoice = company.getInvoice(inputs.invoiceid)
        // you get the idea
    }


GET and POST Parameters
-----------------------

Parameters in the query string or passed in via a `POST` request are available in the `params` map.

    get("/search") {
        def results = SearchEngine.search(params.q)
        // etc.
    }


Templates
---------

Render templates using the `render` method.

    get("/") {
        render "homepage.html"
    }

You can also pass in a map to use in the template.

    get("/page/:pagename") { urlparams ->
        render "page.html", [name: urlparams.pagename]
    }

The template syntax is the same as Groovy's [SimpleTemplateEngine][].

  [SimpleTemplateEngine]: http://groovy.codehaus.org/Groovy+Templates


The Development Server
----------------------

Just give your RatpackApp to the RatpackServlet to start a Jetty container.

    RatpackServlet.serve(app)

The default port is 5000, but you can specify another if you wish.

    RatpackServlet.serve(app, 8080)
