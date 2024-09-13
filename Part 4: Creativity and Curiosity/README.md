### For the first task, I created a simple Flask web application, but using Flask as a backend tool, we can develop an application that helps different people demonstrate their skills or list the platforms they need on their resume or GitHub profile.
The layout is shown below
![image](https://github.com/user-attachments/assets/e4ba7107-757d-4d89-87e3-72bc85980090)

![image](https://github.com/user-attachments/assets/c32036dd-7c8a-42ea-a02f-1daca76c9e17)
With the help of these icons, in a few clicks, the user can add a ready-made image in the version he needs: something darker or lighter. But this is not the final version of this web application. I have some ideas for streamlining the processes and improving the performance of the application, so below I will explain how this can be implemented.

### 1. Website performance

It's not hard to guess that the site will take a long time to load, considering the loading speed of modern sites, according to Google Core Web Vitals, Largest Contentful Paint 2.5 seconds or less is a good page load time. A page that takes 4 seconds to load is acceptable, but no more than that. Therefore, I would reduce the speed of loading the site due to the finalization of the architecture. In my opinion, it would be worth adding an additional page where there is a search box with categories to select an icon theme, an application category, i.e. frontend or backend, or any other development area. After selecting the desired parameters, the user clicks on the search button, thus a smaller number of images will be obtained, which will reduce the loading time.
But that's not all. The images will be loaded in an unspecified order, the one that loads faster will appear first, but you can add a "Lazy Loading" principle to each page of the site. According to this principle, only that part of the site that he sees will be loaded for the user. "Lazy loading" is a pretty good practice that will allow you to increase the speed of the site even more.

### 2. Functionality to simplify the use of the application

Adding each icon separately is not the best solution, so this point can be optimized. An optimization would be that when the user inserts a link, for example, in his profile on GitHub, indicates through a special parameter in the link the names of the images he needs and they are automatically downloaded, after that he can copy the link and use it for his purposes.
In the example below, the parameter “p” is used, and after it the required icons are specified.

```bash
https://tech-icons.com/icons?p=git,github,python
```

Such a feature will make using the application easier and more efficient. Because instead of 3 separate links, it will be possible to add one with the required number of images.

### 2.1 Separate functionality for combining icons

As I described above, it is possible to combine several icons into one link, so it would be a good idea to add functionality on the site itself, where it will be possible to add icons through a convenient web interface, the user needs a few clicks, and then by clicking on a button, get a link with pre-selected parameters .

### 3. Taking advantage of Cloud Run

Given that the application will be hosted on Cloud Run. It has certain advantages that can be useful in the further development of the program. In particular, automatic scaling: if traffic increases, Cloud Run increases the number of containers to serve more requests, and when it drops, the number of containers decreases, which allows you to reduce costs, which is no less important for the business, and also reduce the negative impact on users in terms of overloads site
Because Cloud Run is a serverless service, that means we can pay more attention to infrastructure management, software updates, or server security.
Although these are just ideas and the app is in its early stages and still has a lot of work to do, in the future we can configure domain installation in just a few clicks because Cloud Run automatically provides HTTPS for domains, which ensures a secure connection. It is also convenient that we can switch the revisions we need and redirect traffic from one to another, which can be useful when testing new functionality.

### To summarize the above, these are the ideas I have for improving this program. Since it can be like a pet project, it will provide a great opportunity to try out new and interesting ideas, which will allow you to gain valuable practical experience.
