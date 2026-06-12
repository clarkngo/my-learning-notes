---
title: Open EDX
layout: default
---

### Local Docker 
[https://docs.tutor.edly.io/local.html](Local deployment with Docker Compose)

### Environments
#### http://local.openedx.io
- What it is: The LMS (Learning Management System).
- Who uses it: Learners and Instructors.
- What it’s for: This is the main "student-facing" storefront of your university platform. It's where users browse the course catalog, log into their student dashboards, watch lecture videos, complete assignments, check their grades, and view their certificates.

#### http://studio.local.openedx.io
- What it is: The CMS (Content Management System), universally known as Studio.
- Who uses it: Course Creators, Faculty, and Instructional Designers.
- What it’s for: This is your backend course-building factory. You go here to create course outlines, upload videos, design quizzes, set up grading policies, and configure advanced components like interactive labs. Once you hit "Publish" here, the content instantly pushes live to the LMS (local.openedx.io) for students to see.

#### http://apps.local.openedx.io 
- What it is: The MFAs (Micro-Frontend Applications) portal.
- Who uses it: Everyone (under the hood).
- What it’s for: Modern versions of Open edX are moving away from monolithic pages to sleek, modern single-page apps (built with React). This URL hosts specific modular features like the upgraded Account Profile page, the Learning Experience interface (the actual player inside a course), and discussions. You usually don't type this URL in manually; the LMS will automatically redirect you here when a user clicks on their profile or opens a course.


#### http://meilisearch.local.openedx.io
- What it is: The Search Engine Backend dashboard.
- Who uses it: Administrators (you).
- What it’s for: Open edX uses a fast, lightweight search engine called Meilisearch to power the course catalog search bar and in-course content searching. This URL gives you access to its administrative dashboard to check indexing status, monitor search query performance, and troubleshoot database search configurations. (Note: If you visit this link, it may ask for a master key, which is securely generated inside your Tutor configuration).


### Commands
- Create Super Users
```
tutor local run lms ./manage.py lms createsuperuser
```
- Change password
```
tutor local run lms ./manage.py lms changepassword <username>
```
- Activate user
```
tutor local run lms ./manage.py lms shell -c "from django.contrib.auth import get_user_model; User = get_user_model(); u = User.objects.get(username='clarkngo'); u.is_active = True; u.save(); print('\n*** clarkngo is now ACTIVATED! ***\n')"
```