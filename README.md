# Postr
const express = require("express");
const app = express ();
const express = require("express");
const sequelize = require("sequelize");
const sqlite = require("sqlite3");
const passport = require("passport");
const FacebookStrategy = require("passport-facebook").Strategy;
// const sequelize = new Sequelize("Music", "michael", null, {
    host: "localhost",
    dialect: "sqlite",
    storage: "./Chinook_Sqlite_AutoIncrementPKs.sqlite"
  });
function (app,option){
    if (!options.successRedirect) options.successRedirect = "/account";
    if (!options.failureRedirect) options.failureRedirect = "/login";
  
    return {
      init: function() {
        var env = app.get("env");
        var config = options.providers;
        passport.use(
          new FacebookStrategy(
            {
              clientId: config.facebook[env].appId,
              clientSecret: config.facebook[env].appSecret,
              callbackURL: "/auth/facebook/callback"
            },
            function(accessToken, refreshToken, profile, done) {
              const authId = "facebook:" + profile.id;
              User.findOne({ where: { authId: authId } }, function(err, user) {
                if (err) return done(err, null);
                if (user) return done(null, user);
                User.create({
                  authId: authId,
                  name: profile.displayName,
                  role: "user"
                });
              });
            }
          )
        );
  
        app.use(passport.initialize());
        app.use(passport.session());
      },
      registerRoutes: function() {
        app.get("/auth/facebook", function(req, res, next) {
          passport.authenticate("facebook", {
            callbackURL:
              "auth/facebook/callback?redirect=" +
              encodeURIComponent(req.query.redirect)
          })(req, res, next);
        });
  
        app.get(
          "/auth/facebook/callback",
          passport.authenticate(
            "facebook",
            { failureRedirect: options.failureRedirect },
            function(req, res) {
              res.redirect(303, req.query.redirect || options.successRedirect);
            }
          )
        );
      }
    };
  };
const http = require('http');
const fs = require('fs');
const hostname = 'localhost';
const port = 8080;
app.listen(8080, () =>{
    console.log('Hello World');
});
app.set("port", process.env.PORT ||8080);
const handlebars = require("express-handlebars").create({ defaultLayout: 'main' });
app.engine('handlebars', handlebars.engine);
app.set('view engine', 'handlebars');
const stmt = db.prepare();
app.use((require('body-parser')()));
const options = {
    key: fs.readFileSync(__dirname + 'path_to_private_key'),
    cert: fs.readFileSync(__dirname + 'path_to_cert')
};

https.createServer(options, app).listen(app.get('port'), () => {
 console.log('Express started on port ' + app.get('port') + '.');
});

// const query = 'SELECT Name, Title FROM Album INNER JOIN Artist ON album.ArtistId = artist.ArtistId';
// db.each(query, (err, table) => {
//     if (err) throw err;
//     console.log(table)
// });
// app.post('/', (req,res) => {

app.post('/fullstory', (req,res) => {
    console.log("User: " + req.body.user);
    console.log("Post: " + req.body.post);
    console.log("Passport: " + req.body.passport);
});
app.post("/", function(req, res) {
    res.send("Data can be created using this method (POST).");
    console.log("POST was called.")
  });
  
  app.get("/", function(req, res) {
    res.send("GET is used to read information.");
    console.log("GET was called.")
  });
  
  app.put("/", function(req, res) {
    res.send("Use this (PUT) to update information.");
    console.log("PUT was called.")
  });
  
  app.delete("/", function(req, res) {
    res.send("Only use DELETE to remove the specified target.");
    console.log("DELETE was called.")
  });

  app.get('/account', (req, res) => {
    if(!req.session.passport.user)
      return res.redirect(303, '/unauthorized');
    res.render('account');
  })
