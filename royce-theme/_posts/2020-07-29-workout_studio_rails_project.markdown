---
layout: post
title: "Workout Studio Rails Project"
date: 2020-07-29 20:01:57 +0000
permalink: workout_studio_rails_project
# tags: [JavaScript, Tips]
# featured_image_thumbnail:
featured_image: assets/images/posts/2019/desk.jpg
featured: true
hidden: true
---

### (Sessions Controller Create Route)

For my Rails project I wanted to create a web app that would represent a fictional workout studio - Sweat Workout Studio. I wanted to differentiate between Students and Instructors, such that Students would only see the user side of the web app, while the Instructors have admin privileges and see the admin side.

Students have the ability to register, log in, log out, edit, and delete their information. Students are also able to register for or drop workout classes. Instructors are able to register, log in, log out, edit, and delete their information, but, unlike Students, they are also able to create, edit and delete workout classes and other instructors’ information (admin privileges).

One of the biggest things I struggled with was how to set up my Sessions controller. I wanted Students to be able to log in with email and password and also be able to log in using a third party provider (Google). I also wanted to make sure that only Instructors, and not Students, have access to the admin side of things. I ultimately ended up setting up my Sessions controller create method as follows:

```
class SessionsController < ApplicationController

    def create
        if params[:student]
            @student = Student.find_by(email: params[:student][:email])
            if @student && @student.authenticate(params[:student][:password])
                session[:student_id] = @student.id
                redirect_to student_path(@student.id)
            else
                flash[:message] = "Invalid email or password."
                redirect_to login_path
            end
        elsif params[:instructor]
            @instructor = Instructor.find_by(email: params[:instructor][:email])
            if @instructor && @instructor.authenticate(params[:instructor][:password])
                session[:instructor_id] = @instructor.id
                redirect_to admin_show_instructor_path(@instructor.id)
            else
                flash[:message] = "Invalid email or password."
                redirect_to admin_login_path
            end
        elsif auth[:provider] == "google_oauth2"
            @student = Student.find_or_create_by(email: auth[:info][:email]) do |s|
                s.name = auth[:info][:name]
                s.email = auth[:info][:email]
                s.password = auth[:uid]
                s.password_confirmation = s.password
            end
            session[:student_id] = @student.id
            redirect_to student_path(@student.id)
        end
    end

    private
    def auth
        request.env['omniauth.auth']
    end
end
```

The main idea here is that there are three possibilities to log in:

1. Log in as a Student
2. Log in as an Instructor
3. Log in as a Student through Google

I first check params to see whether a Student or an Instructor is logging in, this is determined based on which object is passed to the log in form. Based on that the individual is authenticated, logged in and redirected to the appropriate page. Note the difference in redirect for Student vs. Instructor. If a Student is trying to log in through Google, the third option kicks in (I am checking for `auth[:provider]` here) and a Student is either found or created by information received from Google and then redirected to the appropriate page.

I limited my login options to either email/password option or Google option for simplicity purposes. This could also work if there are more options to login (such as Facebook, Twitter, etc.). In this case, I would suggest updating line `elsif auth[:provider] == “google_oauth2"` to `elsif auth[:provider]`. I hope this summary helps you when setting up your own Sessions controller for instances where you need multiple login options.
