- **Start Date:** (fill me in with today's date, YYYY-MM-DD)
- **PR:** (leave this empty)
- **Issue:** [#0000](link-to-issue) (remove if no associated issue)
- **Keywords:** (4-8 keywords for page meta data)
- **Summary:** (~60 character explanation of the article content for page meta data)

# Using Node.js with frontend frameworks

> In this module we will be showing how to use node.js with React.js on a Linux environment using terminal.

## Node.js + React.js

>for this section of the documentation, we will be creating a simple Hello-World app. For this tutorial we will be using the terminal.

### Requirements:
  - Node.js
  - React.js



#### Node.js:
We will be using this as a server for our client

#### React.js
This is the framework we will be using to create our user interface. You may refer to the React.js website for the complete documentation and tutorial.



## Step 1:
> We will need to setup out React.js application. The easiest way to this is by using the create-react-app command which bundles all the tools you may need for development. Alternatively, you can start building the application by installing the required tools and packages individually. You may want to read the official React.js documentation for this process but for this tutorial, we will be utilizing the create-react-app command.

> assuming you have already learned about NPM on the previous section of the documentation, We will continue with using NPM to install CRA(create-react-app).

> >npm install create-react-app -g

> Here, we are issuing the "npm" command to install "create-react-app" and the additional flag "-g" is specifying to install "create-react-app" globally. In this sense, we can freely issue the command to create React.js application on any directory in you machine without having to install the package all over again for each project you create.

## Step 2:
> Next step is the creation of the project. you want to navigate through the directory where you want to create your project.

> After navigating to the directory of your choice, then you can issue the command

>>create-react-app hello-world

> In the command, you are basically creating a new React project with the name "hello-world"

> Once done, you should receive a confirmation like:

> >Success! Created hello-world at <your directory>/hello-world
Inside that directory, you can run several commands:

  >>npm start
    Starts the development server.

  >>npm run build
    Bundles the app into static files for production.

  >>npm test
    Starts the test runner.

  >>npm run eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

>>We suggest that you begin by typing:

  >>cd hello-world
  >>npm start

>>Happy hacking!


## Step 3:
>At this point, we have our React app created in the current directory. All we have to to do is to go in the directory and start the app.

>>We can accomplish this by issuing the following comands:

>>cd hello-world

>>npm start

>The application will open a new window and launch the app.

>Alternatively, you can access the app on your localhost and on the default port 3000.

>>http://localhost:3000/

#### Finally:
>Open the application on your favorite editor or IDE and edit the file App.js located in src directory.

>> hello-world/src/App.js

>and to formally end this tutorial, replace the Header 1 with "Hello world" and save.

>Proceed to your app to see the changes.




## Conclusion

> To Summarize things up, we created our React app using the create-react-app command on a terminal. This package can be installed via NPM.

> The create-react-app creates an app with some packages useful for development.

> For a manual setup where you would want a more customized packages, please see the other section of this documentation. 
