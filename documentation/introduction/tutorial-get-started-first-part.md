# How to use Vaadin Flow

Using [Vaadin Flow](https://vaadin.com/flow) is very simple. No extensive knowledge of Java is needed, only basic programming skills are required. This tutorial shows you how to create a single-page web UI. All you need is an [IDE](https://en.wikipedia.org/wiki/Integrated_development_environment), like Eclipse, and JDK 8 installed on your computer.

## 1. Overview

With this document, you can create your first Vaadin Flow application. To complete all four parts of the tutorial, you need about 20 to 60 minutes.

In this tutorial, we build a simple customer management system. It is not a real application; we use an in-memory "back-end", so that you can understand how to hook it to an existing Java based back-end. We cover the basic Vaadin Flow development and you can use the result as a basis for more experiments with Vaadin Flow, such as using add-ons, your own custom look-and-feel (aka theme), adding new views, or optimizing the result for mobile support.

At the end of this tutorial, you will have a page composed of a grid, which has the ability of filtering and also the functionality of adding new customers, deleting customers, and updating the existing customers. The final page will look as follows:

![UI to be built in the tutorial](images/FinishedUI.png)

If you do not want to do the exercise at all, you can also just
[download the final application](https://github.com/vaadin/flow-and-components-documentation/tree/master/tutorial-getting-started) and play
around with it.

### 1.1. Installing the Development Tools

The tutorial uses Java 8, so please ensure that you have an up-to-date JDK 8
installed. Most Linux distributions can use package managers to install JDK8.
Windows and Mac users should download it from [Oracle's Java SE site](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
Also, make sure you have the latest version of your IDE. Eclipse is available in
various packages; be sure to download the **Eclipse IDE for Java EE Developers**
from [eclipse.org](http://www.eclipse.org/downloads/).

### 1.2. Starting with the Project Base

As the starting point for the application, we use a Vaadin Application Starter, called Project Base for Vaadin 10. Project Bases are project stubs that have dependencies set up, and may include some example code. The steps of creating a Vaadin Application are as follows:
1. Start by pointing your browser at [Vaadin Start](https://vaadin.com/start)

2. Select the Project Base for V10 by clicking it

3. For Group ID::Give `com.vaadin.starter.skeleton`

4. For App Name::Give `my-app`

5. Click download button to download the preconfigured project stub as a ZIP file. (If the download button is not active, you should log in to download the starter).

6. The name of the downloaded ZIP package should be `my_app.zip`. Locate it from your hard drive and extract it to a folder of your preference.

7. Fire up Eclipse, and from the file menu, select import. From the select window choose Existing Maven Project from Maven section. Select the next button. Select the Browse button to choose your project and finally click the finish button.

If this is your first Vaadin app, opening the project might take a while, depending on the speed of your network, as Vaadin libraries and other dependencies are being downloaded. Maven caches them on your local file system, so creating your next Maven-based Vaadin project will be much faster.

Right click on the newly created project in Project Explorer and choose "Run as > Maven Install". This initiates a full build of your application and finally creates a [WAR](https://en.wikipedia.org/wiki/WAR_(file_format)) file into a directory named target in your project folder. You can deploy the WAR file to your application server when you go to production; but during the development, we'll use Jetty which is provided through Maven and so, there is no need to install an application server. The first build might take a while, as Maven might need to download some additional modules.

|-----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tip | For the Maven compilation to work you need a JDK to be configured in your Eclipse in "Window > Preferences > Java > Installed JREs > Add...​". This step is necessary at least on Windows, if you are using a fresh installation of Eclipse or for some other reason haven't configured a JDK to your Eclipse. The JDK by default installs to \\Program Files\\Java on Windows. You can make JDK the default JRE for your Eclipse. |

While the build is running, let us have a look at what you got from the App Starter. You can browse your project resources from the tree structure in the Project Explorer. The pom.xml Maven file on the top level contains settings for building your project and declares the used dependencies. The Vaadin UI code is in src/main/java.

The Java code of the application stub main view can be found in the
MainView.java file. Let us take a look at it to see how it works.

```java
/**
 * The main view contains a button and a click listener.
 */
@Route("")
public class MainView extends VerticalLayout {

    public MainView() {
        Button button = new Button("Click me",
                event -> Notification.show("Clicked!"));
        add(button);
    }
}
```

The annotation, @Route(""), tells Vaadin Flow to direct the root URL to this view.
In the main view, we have a button and when this button is clicked, a notification is shown. The logic is attached to the button with a simple Java 8 lambda expression, and finally the button is added into the main view.

To test your first Vaadin application, right-click on the project and choose "Debug as ▸ Maven build..."​. The debug mode is slightly slower than the basic run mode, but it often helps you to figure out what is happening in your application.
In the run configuration dialog, type `Debug in Jetty` to the Name input and `jetty:run` to the Goals input.
Generating a Maven launch for `jetty:run` target

![Generating a Maven launch for <code>jetty:run</code> target](images/Jetty-Run.png)

Now click Debug to continue. This will download Jetty, a small Java web server (if not cached to your local Maven repository), and use it to host your application. Once the server has started, point your browser to the URL http://localhost:8080 to see the application in action!

If you make changes to the code, Jetty will notice the changes and in a couple of seconds, most changes are automatically deployed. Reloading the page in your browser will show the changes.

|-----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tip | In some cases your JVM might not allow injecting changes on the fly. In these cases, Eclipse will complain about "Hot Code Replace Failed". Just choose to restart the server to get the latest changes. Many Java developers use a commercial tool called [JRebel](http://zeroturnaround.com/software/jrebel/) to make code replacement work better. |

Mastering the usage of the Java debugger is also handy to better understand how your application actually works and finding causes for any bugs. As Vaadin Flow is Java code, you can use all of Java's debugging tools. Double-click on the line number in the Java editor, for example of the following line in the main view constructor:

```java
Button button = new Button("Click me",
                event -> Notification.show("Clicked!"));
```

Doing so adds a breakpoint to the selected line. If you then reload the page in your browser, the execution of the application will stop on that line. Eclipse will ask you to enter to *Debugging perspective* . That way you can step through the execution and inspect the variables. Clicking on the *play* icon in the toolbar will continue the execution. Double-click the same line again to remove the breakpoint.
Execution in a break point in the button click listener

![Execution in a break point in the button click listener](images/Debugging.png)

Clicking the red square in the Console view will terminate the Jetty server process. You can restart it easily from the run/debug history. You can find that from the small down arrow next to the green play button or bug button (for the debug mode) in the toolbar. Alternatively, you can use the main menu "Run > Run history/Debug history > Debug in Jetty".
To get back to the *Java EE Perspective* , an Eclipse mode designed for editing Java web app code, click the Java EE button in the toolbar.

Good job! Let's move on and continue by adding a data grid and list some entities in the grid in the next tutorial section.
