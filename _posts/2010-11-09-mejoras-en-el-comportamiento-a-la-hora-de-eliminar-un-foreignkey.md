---
redirect_from: "/archivos/2010/11/09/mejoras-en-el-comportamiento-a-la-hora-de-eliminar-un-foreignkey/"
author: milmazz
comments: true
date: 2010-11-09 19:51:35
layout: post
slug: mejoras-en-el-comportamiento-a-la-hora-de-eliminar-un-foreignkey
title: Mejoras en el comportamiento a la hora de eliminar un ForeignKey
wordpress_id: 313
categories:
- django 
tags:
- django
- Python
---

![Logo de Django ](/images/2010-11-09-mejoras-en-el-comportamiento-a-la-hora-de-eliminar-un-foreignkey/djangove.png) Cuando un objeto referenciado por una clave foránea (`ForeignKey`) es eliminado, [Django][] por omisión emula el comportamiento de la sentencia SQL `ON DELETE CASCADE` y también se elimina el objeto que contiene el _ForeignKey_.

A partir de la versión 1.3 de Django el comportamiento descrito en el párrafo anterior puede ser sobreescrito al especificar el argumento `on_delete`. Por ejemplo, si usted permite que una clave foránea pueda ser _nula_ y usted desea que sea establecida a `NULL` cuando un objeto referenciado sea eliminado:

{% highlight python %}
user = models.ForeignKey(User, blank=True, null=True, on_delete=models.SET_NULL)
{% endhighlight %}

Los posibles valores para el argumento `on_delete` pueden encontrarse en django.db.models:

* CASCADE: Eliminación en cascada, el comportamiento por omisión.
* PROTECT: Prevee la eliminación del objeto referenciado al lanzar una excepción del tipo: `django.db-IntegrityError`.
* SET_NULL: Establece la clave foránea a `NULL`, esto solo es posible si el argumento `null` es `True`.
* SET_DEFAULT: Establece la clave foránea a su valor por omisión, tenga en cuenta que un valor por omisión debe ser establecido.
* SET(): Establece el valor del `ForeignKey` indicado en `SET()`, si una función es invocada, el resultado de dicha función será el valor establecido.
* DO_NOTHING: No tomar acciones. Si el gestor de base de datos requiere integridad referencial, esto causará una excepción del tipo `IntegrityError`.

A continuación un par de ejemplos de esta nueva funcionalidad:

{% highlight python %}
# models.py
from django.db import models

class Author(models.Model):
    nickname = models.CharField(max_length=32)

    def __unicode__(self):
        return self.nickname

class Post(models.Model):
    author = models.ForeignKey(Author, blank=True, null=True, on_delete=models.SET_NULL)
    title = models.CharField(max_length=128)

    def __unicode__(self):
        return self.title
{% endhighlight %}

Nuestra sesión interactiva con el API sería similar a la siguiente:

	$ python manage.py shell

{% highlight python %}
>>> from ondelete.models import Author, Post

>>> Author.objects.all()
[]
# Creamos el autor
>>> author = Author(nickname='milmazz')
# Guardamos el objeto en la base de datos al usar de manera explícita el método save()
>>> author.save()

# Obtenemos el autor en base a su id
>>> Author.objects.get(pk=1)
<Author: milmazz>

# Creamos par de artículos
>>> article1 = Post(author=author, title="Article 1")
>>> article1.save()

>>> article2 = Post(author=author, title="Article 2")
>>> article2.save()

>>> for article in Post.objects.all():
     print("%s by %s" % (article.title, article.author))
Article 1 by milmazz
Article 2 by milmazz

# Eliminamos el autor
>>> author.delete()

>>> for article in Post.objects.all():
    print("%s by %s" % (article.title, article.author))
Article 1 by None
Article 2 by None
{% endhighlight %}

Un segundo ejemplo, ahora haciendo uso del valor `SET()` en el argumento `on_delete`:

{% highlight python %}
# models.py
from django.db import models
from django.contrib.auth.models import User

def get_superuser():
    return User.objects.get(pk=1)

class Post(models.Model):
    user = models.ForeignKey(User, on_delete=models.SET(get_superuser))
    title = models.CharField(max_length=128)

    def __unicode__(self):
        return self.title
{% endhighlight %}

Nuestra sesión interactiva con el API sería similar a la siguiente:

	$ python manage.py shell

{% highlight python %}
>>> from ondelete.models import Post
>>> from django.contrib.auth.models import User

>>> User.objects.all()
[<User: milmazz>]
# Creamos un nuevo usuario
>>> author = User(username='milton')
# Guardamos el objeto en la base de datos, 
# de manera explícita al invocar el método save()
>>> author.save()
# Vista de los usuarios registrados en la base de datos
>>> User.objects.all()
[<User: milmazz>, <User: milton>]

# Creamos par de artículos
>>> article1 = Post(user=author, title="Article 1")
>>> article1.save()

>>> article2 = Post(user=author, title="Article 2")
>>> article2.save()

>>> for article in Post.objects.all():
     print("%s by %s" % (article.title, article.user))
Article 1 by milton
Article 2 by milton

# Eliminamos el usuario 'milton'
>>> author.delete()

>>> for article in Post.objects.all():
    print("%s by %s" % (article.title, article.user))
Article 1 by milmazz
Article 2 by milmazz
{% endhighlight %}

[Django]: http://www.djangoproject.com
