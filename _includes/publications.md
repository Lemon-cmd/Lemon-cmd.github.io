<h1 id="publications"></h1>

<h2 style="margin: 60px 0px -15px;">Selected Works <temp style="font-size:22px;">(* denotes Equal Contribution)</temp><temp style="font-size:22px;"></temp></h2>


<div class="publications">
<ol class="bibliography">

{% for link in site.data.publications.main %}

<li>
<div class="pub-row">
  <div class="col-sm-3 abbr" style="position: relative;padding-right: 15px;padding-left: 15px;">
    <img src="{{ link.image }}" class="teaser img-fluid z-depth-1" style="width=100;height=40%">
            {% if link.conference_short %}
            <abbr class="badge">{{ link.conference_short }}</abbr>
            {% endif %}
  </div>
  <div class="col-sm-9" style="position: relative;padding-right: 15px;padding-left: 20px;">
      <div class="title"><a href="{% if link.page %}{{ link.page }}{% else %}{{ link.pdf }}{% endif %}">{{ link.title }}</a></div>
      <div class="author">{{ link.authors }}</div>
      <div class="periodical"><em>{{ link.conference }}</em>
      </div>
    <div class="links">
      {% if link.code %} 
      <a href="{{ link.code }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Code</a>
      {% endif %}
      {% if link.slides %} 
      <a href="{{ link.slides }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Slides</a>
      {% endif %}
      {% if link.code_jax %} 
      <a href="{{ link.code_jax }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Code (Jax)</a>
      {% endif %}
      {% if link.code_torch %} 
      <a href="{{ link.code_torch }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Code (Torch)</a>
      {% endif %}
      {% if link.page %} 
      <a href="{{ link.page }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Project Page</a>
      {% endif %}
      {% if link.talk_1 %} 
      <a href="{{ link.talk_1}}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Recording 1</a>
      {% endif %}
      {% if link.talk_2 %} 
      <a href="{{ link.talk_2}}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Recording 2</a>
      {% endif %}
      {% if link.talk_3 %} 
      <a href="{{ link.talk_3}}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Recording 3</a>
      {% endif %}
      {% if link.data %} 
      <a href="{{ link.data }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Dataset</a>
      {% endif %}
      {% if link.bibtex %} 
      <a href="{{ link.bibtex }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">BibTex</a>
      {% endif %}
      {% if link.notes %} 
      <strong> <i style="color:#e74d3c; font-weight:600">{{ link.notes }}</i></strong>
      {% endif %}
      {% if link.others %} 
      {{ link.others }}
      {% endif %}
    </div>
  </div>
</div>
</li>

<br>

{% endfor %}

</ol>
</div>


