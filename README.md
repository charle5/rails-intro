my notes:
i figured out how to do this with the help of [this](http://railscasts.com/episodes/228-sortable-table-columns) railscast.
the most difficult part was figuring out haml syntax instead of using erb (which i might actually want to do after the assignment is submitted.)

to get the sorts to work i created a helper method called 'sortable' in application_helper.rb, and created and declared two private helper methods (``sort_column` and `sort_direction`) in the movies controller, making them available to the view.

the purpose of the helper method `sortable` is to obviously sort whatever column is clicked, and to also apply the correct styling. sortable takes two arguments: 1. the name of the column as it appears in the schema; 2. optionally the column header text (in case it's different than the name of the column).

`sort_column` checks to see if the movie object's array of column names (i.e. one of the fields in the movie model) includes a sort param. otherwise it defaults to `name`. this is done to protect against SQL injection.

`sort_direction` does basically the same thing for the same reasons as `sort_column` but instead checks for the presences of `asc` or `dsc`, defaulting to `asc`. 

to get the styling to show up for sorted columns i had to add stuff to application.css. but the real magic happens by taking out `link_to` from the view, putting it in the sortable method, passing it the titleized title followed by two hashes: 1. sort and direction; 2. the css class (`current` in this case).

so what happens is when the column is clicked, the column title gets titleized, the `css_class` is created by sanitizing the params and applying `current` plus whatever sort direction, and all this gets passed to `link_to`.  


HW2: RAILS INTRO - ADD FEATURES TO ROTTENPOTATOES
---------

In this homework you will add a feature to an existing simple Rails app and deploy the result publicly on the Heroku cloud hosting service. We will run live integration tests against your deployed version.

General advice:  This homework involves modifying RottenPotatoes in various ways. Git is your friend: commit frequently in case you inadvertently break something that was working before! That way you can always back up to an earlier revision, or just visually compare what changed in each file since your last “good” commit.

Remember, commit early and often!

Preparation: If you've never used Rails before please follow these screencasts to work through chapter 4 of the ESaaS textbook to create a rottenpotatoes rails app from scratch.  This assignment assumes that you have a working version of RottenPotatoes running locally and deployed on Heroku. Please follow these steps to make an initial deployment to Heroku:

Clone the assignment skeleton:

    git clone https://github.com/saasbook/rails-intro

Switch to the rails_intro directory

    cd rails-intro

Now you need to install various libraries with the bundle command

    bundle install --without production

Next you will need to run some commands to set up the database locally and add some data

    bundle exec rake db:migrate

    bundle exec rake db:seed

Now you can test the app locally by running the following and navigating to http://localhost:3000/movies in your browser

    rails s

In order to deploy to Heroku you will need to sign up for a free Heroku account at https://id.heroku.com/signup and follow the instructions to create your account. Assuming you have Heroku Toolbelt installed (it should be pre-installed on the VM) you now need to set up ssh keys to securely talk to Heroku.  The three basic commands you need are the following, but see the Heroku page for more details.

    ssh-keygen -t rsa

    heroku login

    heroku keys:add

Note that this is an area that a lot of people get seriously stuck, particularly if they have existing ssh keys.  If you are on a fresh VM you should be fine.  If you do get stuck here, please do ask in the forums.

Once you have your heroku keys set up correctly you can create a heroku instance from the rottenpotatoes directory

    heroku create

Now we can use git to deploy our code to the Heroku server in the cloud

    git push heroku master

Note, if you see a warning such as:

    The authenticity of host 'heroku.com (50.19.85.132)' can't be established.
    RSA key fingerprint is 8b:48:5e:67:0e:c9:16:47:32:f2:87:0c:1f:c8:60:ad.
    Are you sure you want to continue connecting (yes/no)? 
    Please type 'yes' or 'no':

This is normal - go ahead and type yes then hit 'ENTER to add heroku to the list of known remote computers.

Next we need to tell the cloud instance to prepare the database:

    heroku run rake db:migrate

We need to tell the cloud instance to add some data to the database:

    heroku run rake db:seed

Now you can navigate to the heroku url that heroku create printed to the console and see your app running in the cloud.

    heroku open

You can find more details on git and Heroku in the appendices of the ESaaS textbook.
