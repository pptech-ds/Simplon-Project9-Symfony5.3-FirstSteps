# Install For Ubuntu Users

## Install PHP (5.6, 7.x, 8.0) on Ubuntu Using PPA

1. First start by adding Ondřej Surý PPA to install different versions of PHP – PHP 5.6, PHP 7.x, and PHP 8.0 on the Ubuntu system.

```console
sudo apt install python-software-properties
sudo add-apt-repository ppa:ondrej/php
```

2. Next, update the system as follows.

```console
sudo apt-get update
```

3. Now install different supported versions of PHP as follows. For Apache Web Server (example here for 7.4)

```console
sudo apt install php7.4
```

4. To install any PHP modules, simply specify the PHP version and use the auto-completion functionality (tab) to view all modules as follows.

```console
sudo apt install php7.4(tab)
```

5. Finally, verify your default PHP version used on your system like this.

```console
php -v
```

6. You can set the default PHP version to be used on the system with the update-alternatives command, after setting it, check the PHP version to confirm as follows.

```console
sudo update-alternatives --set php /usr/bin/php5.6
```

7. To set the PHP version that will work with the Apache web server, use the commands below. First, disable the current version with the a2dismod command and then enable the one you want with the a2enmod command.

```console
sudo a2dismod php7.4
```

```console
sudo a2enmod php7.4
```

8. After switching from one version to another, you can find your PHP configuration file, by running the command below.

```console
sudo update-alternatives --set php /usr/bin/php7.4
php -i | grep "Loaded Configuration File"
```

## Install composer

All details in [this page](https://getcomposer.org/download/)  
Move into a temporary directory and the following command lines:

```console
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
sudo mv composer.phar /usr/local/bin/composer
composer self-update
```

## Install Symfony

All details in [this page](https://symfony.com/download)

1. Move into a temporary directory and the following command lines:

```console
wget https://get.symfony.com/cli/installer -O - | bash
```

2. Add Symfony command into your env (for our needs choose the 3rd choice):

- Use it as a local file:

  ```console
  /home/pras/.symfony/bin/symfony
  ```

- Or add the following line to your shell configuration file:

  ```console
  export PATH="$HOME/.symfony/bin:$PATH"
  ```

- Or install it globally on your system:
  ```console
  mv /home/pras/.symfony/bin/symfony /usr/local/bin/symfony
  ```

## For our project specific needs

1. update and upgrade your env:

```console
sudo apt-get update
sudo apt-get upgrade
```

2. get you php version

```console
php -v
```

3. add missing modules (change X depending of your PHP version), for example here "PHP 7.4.20 (cli)"

```console
sudo apt-get install php7.4-sqlite3
sudo apt-get install php7.4-mbstring
sudo apt-get install php7.4-mysql
```

# Projects: Introduction
We are going to create a blog where we are going to manage different type of users, and usual blog applications, like posting or commenting an article. 
Here is the CDM for our project:  
![image](https://user-images.githubusercontent.com/61125395/122682218-7336d500-d1f8-11eb-8645-eca4623cf50e.png)


# Projects: Creation and Management

1 Create a new project using symfony CLI
  - For our porject  
```console
symfony new Simplon-Project9-Symfony5.3-Blog --version=5.3 --full
```
![image](https://user-images.githubusercontent.com/61125395/122682357-20a9e880-d1f9-11eb-8abb-38c98d738f2e.png)


Some other methods to create projects:  
  - Create a project using maintained branch  
```console
symfony new <project_name> --full
```
  - Create a project using LTS (Long Term Support)  
```console
symfony new <project_name> --version=lts
```
  - Create a project using developement version  
```console
symfony new <project_name> --version=next
```
  - Create symfony demo project to have an example
```console
symfony new <project_name> --demo
```

3. Setting up an Existing Symfony Project

```console
git clone <project_git_path>
cd <project_name>/
composer install
```

4. Running Symfony Applications

```console
cd <project_name>/
symfony server:start
```

# Projects: About Routes

1. Create controller using maker  
```console
php bin/console make:controller
```
Specifiy the name you want for the controller, for our project we are going to create first of all, 2 controllers, one to manage posts, and another one to manage pages
  ![image](https://user-images.githubusercontent.com/61125395/122682413-79798100-d1f9-11eb-8d6f-cbcac0181c54.png)   
  And we can see that 2 files were created, the controller("src/Controller/<Name>Controller.php") and the view("templates/<name>/index.html.twig")  
  We can check thoses files, for example the ones for posts:  
  The controller:  
  ```php
  <?php

  namespace App\Controller;

  use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
  use Symfony\Component\HttpFoundation\Response;
  use Symfony\Component\Routing\Annotation\Route;

  class PostController extends AbstractController
  {
      /**
       * @Route("/post", name="post")
       */
      public function index(): Response
      {
          return $this->render('post/index.html.twig', [
              'controller_name' => 'PostController',
          ]);
      }
  }
  ```  
  The view:  
  ```php
  {% extends 'base.html.twig' %}

  {% block title %}Hello PostController!{% endblock %}

  {% block body %}
  <style>
      .example-wrapper { margin: 1em auto; max-width: 800px; width: 95%; font: 18px/1.5 sans-serif; }
      .example-wrapper code { background: #F5F5F5; padding: 2px 6px; }
  </style>

  <div class="example-wrapper">
      <h1>Hello {{ controller_name }}! ✅</h1>

      This friendly message is coming from:
      <ul>
          <li>Your controller at <code><a href="{{ '/mnt/c/wamp64/www/simplon/symfony/Simplon-Project9-Symfony5.3-Blog/src/Controller/PostController.php'|file_link(0) }}">src/Controller/PostController.php</a></code></li>
          <li>Your template at <code><a href="{{ '/mnt/c/wamp64/www/simplon/symfony/Simplon-Project9-Symfony5.3-Blog/templates/post/index.html.twig'|file_link(0) }}">templates/post/index.html.twig</a></code></li>
      </ul>
  </div>
  {% endblock %}
  ```
  We can check the page using the defined route, in our post controller, this is "/post", so we need to go from our browser here ""

# Download theme to integrate into symfony projects

1. Theme to use: https://startbootstrap.com/theme/clean-blog
![image](https://user-images.githubusercontent.com/61125395/122299821-c78f3b80-cefe-11eb-857e-7c966a2549a2.png)  

2. Let's make 2 controllers to manage posts and pages  
Into post controller we can have one method to show all posts and another one to show one specific post, and into
page controller we can have one method to manage about page and another one to manage contact page.  
To create controllers, into your shell:  
```php
php bin/console make:controller
```
and choose, **Page** and **Post** as names with keeping Upper Case for first letter, class standard definition naming convention.  

3. The maker will create controllers and views  
We can create necessary routes, we decided to have 2 routes for **Post** 
```php
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class PostController extends AbstractController
{
    /**
     * @Route("/", name="post_home")
     */
    public function home(): Response
    {
        return $this->render('post/index.html.twig', [
            'bg_image' => 'home-bg.jpg', 
        ]);
    }

     /**
     * @Route("/post/{id}", name="post_view", methods={"GET"}, requirements={"id"="\d+"})
     */
    public function view($id): Response
    {
        return $this->render('post/view.html.twig', [
            'post' => [
                'title' => 'Le titre de l\'article',
                'content' => 'Le super contenu de notre article'
            ],
            'bg_image' => 'post-bg.jpg', 
        ]);
    }
}
```
and also 2 routes for **Page**
```php
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class PageController extends AbstractController
{
    /**
     * @Route("/about", name="page_about")
     */
    public function about(): Response
    {
        return $this->render('page/about.html.twig', [
            'page_title' => 'About',
            'bg_image' => 'about-bg.jpg', 
        ]);
    }

    /**
     * @Route("/contact", name="page_contact")
     */
    public function contact(): Response
    {
        return $this->render('page/contact.html.twig', [
            'page_title' => 'Contact',
            'bg_image' => 'contact-bg.jpg', 
        ]);
    }
}
```



