django-page-search
==================

Searching for content on the pages of your website

BSD-licensed menu tools for Django, built by Charles Garrocho <http://charlesgarrocho.github.io>

django-page-search provides a simple search content in your html pages. The research just does not work in your templates.

Installation:
-----------------------------

1. ``pip install git+git://github.com/CharlesGarrocho/django-page-search``

Configuration:
-----------------------------

2. Add ``page-search`` to your ``INSTALLED_APPS``

Usage:
-----------------------------

3. Add ``import page-search`` to your ``views.py`` from your app.

4. call method to search content in ``views.py``:

                rq = page_search.search(request)
    			return render_to_response('search.html', context_instance=rq)

You can also spend the amount of characters for content validation.

				rq = page_search.search(request, quantity_characters=4)

5. In their search page:

				{% extends 'base.html' %}
				  {% block body_page %}
					  <div>
					    {% if quantity_characters_check != "OK" %}
					        <h3>The search must have at least {{ quantity_characters }} characters.</h3>
					    {% elif argument_check != "OK" %}
					        <h3>Fill in the field with the text being searched...</h3>
					    {% elif content_check != "OK" %}
					        <h3>No results for search for: {{ search_words }} </h3>
					    {% else %}
					        <h3>Search Results for: {{ search_words }} </h3>
					        <br/>
					        {% for item in search_result %}
					          {% if item.TEXT.strip|striptags|truncatewords:50 %}
					            <div>
					              <a class="lead" href="{{ item.URL }}"> {{ item.TEXT.strip|striptags|truncatewords:50 }}</a>
					            </div>
					            </br>
					          {% endif %}
					        {% endfor %}
					    {% endif %}
					  </div>
				{% endblock %}

6. Finally, the html pages may contain some words that you would like to substituílas before the survey, such as: ``{{ SITE_NAME }}`` or ``[at]`` or ``[dot]``. For this, you must pass a dictionary as an argument to the method to search within the dictionary. Inside you will put the key (the word to be searched) and value (the word that will replace).

				dict_text = {"{{ SITE_NAME }}":"Charles", "[at]":"@", "[dot]":"."}
				rq = page_search.search(request, replace_text=dict_text)
    			return render_to_response('search.html', context_instance=rq)