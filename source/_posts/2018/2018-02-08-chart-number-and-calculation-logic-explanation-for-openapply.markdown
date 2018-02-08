---
layout: post
title: "Chart number and calculation logic explanation for OpenApply"
date: 2018-02-08T15:24:09+08:00
comments: true
external-url: 
categories: [OpenApply]
---

I plan to rewrite the [OpenApply](https://demo.openapply.com/) all the chart, so need list the calculation logic as below first.

## Dashboard

The dashboard provides a quick overview of most common key number for the school.

### Enquiries

Enquiries provide a month by month overview for inquiry and it's submitted applicant forms number.

Monthly inquiry students define as the new students created at that month.

Monthly conversion students define as the student who submits the application form that month.

So normally, such gap between inquiries and conversion should not very large, otherwise, it's indicate maybe your application form is too complex and many students don't finish it.

### Applicants

Applicants will list the students' number per day, the students is the one who was being marked at the applied status in that day, the mark effective date can be different with actually the marking day.

Applicants date range always starts at Month Day 1.

### Enrolment

Enrolment chart will display the count number of enrolled student per that month. The enrolled day is students status histories enrolled record effective on date.

## Re-Enrolment

Re-enrolment help you manage the re-enrolment student status. there is 3 status which will be recorded in student reenrolment status.

Pending means email already sent, but student no response.
Confirmed means student confirm the re-enrolment for your select academic year.
Declined means student read the email but decide not continue attending the school at the select academic year.

If the student is eligible to attend the select academic year, but no student reenrolment status record, it will belong to Not sent.

## Analytics

### Enquiry

Based on filter condition, to analyze the company, nationality by reference source or grade distribution.

The time range always the academic year.

### Conversion Funnel

Conversion Funnel focus on the enroll perspective.

The left part is the drop down student status for the select academic year, the Pending number is 100% percent, then all other status is based on that percent.

In demo side, as the enrolled student in 25%, which means only 1/4 student can be enrolled amount all inquiry student.

Center part pie chat compare the declined and enrolled students for that year.

Right small pie chat display sub status for select status students, if the school does not configure their own sub-status, it will leave blank.

### Enrolment

This part no chart, so intentionally leave blank.

### Checklist

The checklist will give an overview of checklist status per academic year and per program.

Pending means those students not complete the checklist item.
Partial means those students have already filled the forms, but not submit yet.
Complete means those students finish such checklist item.

Click any part of rectangle box will display detail students in such status in the checklist item.

You can filter to analyze students as the checklist completion data source.

### Geography

It will analyze the student country, if the student country is blank, will take parents country instead.

### Nationality

It will analyze the student's nationality.

### Languages

It will analyze the student's language.
