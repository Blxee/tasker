# Tasker

An easy to use and simple full-stack web application for managing tasks.

### Requirements:

Before running the app, make sure you have the following tools installed:

* [__Git__](https://git-scm.com/)
* [__NodeJS + NPM__](https://nodejs.org/en)
* [__PHP__](https://www.php.net/)
* [__Laravel__](https://laravel.com/docs/11.x/installation#installing-php)
* [__MySQL__](https://www.mysql.com/)

### Getting started 🚀:

* First clone this repository to your computer using:

```bash
git clone https://github.com/Blxee/tasker.git
```

> Please install git if you don't have it already!

* cd to the directory:

```bash
cd tasker
```

* Install npm packages:

```bash
npm install
```

* Finally start the application with the following:

```bash
npm run build
php artisan serve
```

Now you can visit [this link](http://127.0.0.1:8000) to test the app.

### API routes:

|route|method|description|
|-----|------|-----------|
|`/api/tasks?page=<n>`|__GET__|Retrieves all available tasks in pages of 10|
|`/api/tasks`|__POST__|Creates a new task with title and description|
|`/api/tasks/:id`|__PUT__|Updates a task using its `id`|
|`/api/tasks/:id`|__DELETE__|Deletes a task using its `id`|
