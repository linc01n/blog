---
layout: post
title: "Django Admin: Auto Select Current User"
date: 2011-10-18 17:38
comments: true
categories: [coding, python, django]
---

## Background

There are often times in developing a CMS need to mark down who created the record. Django provide a nice admin interface for us. But when we only use the default, user need to select who they are by themselves. We want to auto select for them and they can't pretend to be others.

## code

In the django models.py, we have the following:

{% codeblock models.py lang:python %}
class Notes(models.Model):
    title = models.CharField(max_length=255)
    content = models.TextField()
    date = models.DateField()
    created_by = models.ForeignKey(User, related_name="Created By")
{% endcodeblock %}

We want the created_by to be set automatically for the logged in user.

So in our admin:
{% codeblock admin lang:python %}
class NotesAdmin(admin.ModelAdmin):
    def formfield_for_foreignkey(self, db_field, request, **kwargs):
        if db_field.name == 'created_by':
            kwargs['queryset'] = User.objects.filter(username=request.user.username)
        return super(NotesAdmin, self).formfield_for_foreignkey(db_field, request, **kwargs)

    def get_readonly_fields(self, request, obj=None):
        if obj is not None:
            return self.readonly_fields + ('created_by',)
        return self.readonly_fields

    def add_view(self, request, form_url="", extra_context=None):
        data = request.GET.copy()
        data['created_by'] = request.user
        request.GET = data
        return super(NotesAdmin, self).add_view(request, form_url="", extra_context=extra_context)

admin.site.register(Notes, NotesAdmin)
{% endcodeblock %}

## explanation

The add_view function help us to select the current user from the drop-down list.

The formfield_for_foreignkey help us to filter out other users.

By combining this two function we get:

![Created By](/images/created_by.jpg)

The last get_readonly_fields function make this field can't be edit after the creation.
