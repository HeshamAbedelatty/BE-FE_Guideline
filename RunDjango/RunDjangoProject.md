# Django Project Setup Guide

## Table of Contents
1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites)
3. [Cloning the Project](#3-cloning-the-project)
4. [Setting Up a Virtual Environment](#4-setting-up-a-virtual-environment)
5. [Installing Dependencies](#5-installing-dependencies)
6. [Database Migrations](#6-database-migrations)
7. [Running the Server](#7-running-the-server)
8. [Common Issues](#8-common-issues)

## 1. Introduction
This guide will walk you through the steps to set up and run the Django project locally on your machine. By the end, you'll have a working environment ready for development and testing.

## 2. Prerequisites
Before starting, ensure you have the following installed:
- **Python 3.x** (Check the version with `python --version`)
- **Git** (Check with `git --version`)
- **pip** (Python package manager, check with `pip --version`)
  
Optional but recommended:
- **Virtualenv** for creating isolated Python environments (`pip install virtualenv`)

## 3. Cloning the Project
1. Navigate to the directory where you want to clone the project.
2. Run the following command in your terminal:
   ```bash
   git clone <repository_url>
   ```
   Replace `<repository_url>` with the GitHub URL of the project.

3. Navigate into the project directory:
   ```bash
   cd <project_name>
   ```

## 4. Setting Up a Virtual Environment
1. Create a virtual environment by running the following command:
   ```bash
   python -m venv venv
   ```
2. Activate the virtual environment:

   - **On Windows:**
     ```bash
     venv\Scripts\activate
     ```
   - **On macOS/Linux:**
     ```bash
     source venv/bin/activate
     ```

## 5. Installing Dependencies
Once the virtual environment is activated, install the project dependencies using the following command:
```bash
pip install -r requirements.txt
```
If "requirements.txt" is not exists, so you can run:
```bash
pip install django djangorestframework pillow              # you can add any libraries if it is needed
```

## 6. Database Migrations
After installing the dependencies, you'll need to apply the database migrations:

1. Run the migration commands:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

2. If you want to create an admin user, use the following command:
   ```bash
   python manage.py createsuperuser
   ```

## 7. Running the Server
To start the development server, run the following command:
```bash
python manage.py runserver
```
The server will start on `http://127.0.0.1:8000/` by default.

## 8. Common Issues
- **Virtual environment activation issue**: Ensure that you are using the correct command to activate the virtual environment based on your OS.
- **Missing dependencies**: Run `pip install -r requirements.txt` if any dependencies are missing.

---

