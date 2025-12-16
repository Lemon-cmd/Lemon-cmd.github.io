{% for link in site.data.navigation.main %}
    <a class="normal" href="{{ link.url | relative_url }}">{{ link.title }}</a>
{% endfor %}

