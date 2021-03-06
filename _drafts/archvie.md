<blockquote class="col-12">
  The default definition for SRID 3735 in PostGIS has been updated internally to reflect the ESRI ITRF00 transformation - 
  <pre>+proj=lcc +lat_1=40.03333333333333 +lat_2=38.73333333333333 +lat_0=38 +lon_0=-82.5 +x_0=600000 +y_0=0 +ellps=GRS80 +towgs84=-0.9956,1.9013,0.5215,0.025915,0.009246,0.011599,-0.00062 +units=us-ft +no_defs</pre>
</blockquote>

<section class="col-12">
<h1>Categories</h1>
</section>

{% for category in categories %}
{% assign sorted_posts = category[1] | sort: "title" %}
<h2 class="col-12" style="padding: 0 1rem;text-transform:capitalize;" id="{{ category[0] }}">{{ category[0] }}</h2>
{% for post in sorted_posts %}
{% if post.name != "index.html"%}
<article class=" js-cards column col-3 col-xl-6 col-sm-12" style="display: none;">
  <div class="card {%if category[0] != "archive"%}active{%endif%}" data-link="{{site.baseurl}}{{post.url}}">
    <div class="card-header">
      <a href="{{site.baseurl}}{{post.url}}" style="color:inherit;">
        <h3>{{post.title | replace: "_posts/", ""}}</h3>
      </a>
    </div>
    <div class="card-body">
      <a href="{{site.baseurl}}{{post.url}}" style="color:inherit;">
      {{ post.subtitle }}
      </a>
    </div>
    <div class="card-footer">
      <div class="columns" style="opacity: 0.8">
        <div class="column col-9">
          {% for tag in post.tags %}<a class="js-tags" href="#{{tag}}"><span class="chip bg-dark">{{tag}}</span></a>{% endfor %}
        </div>
        <div class="column col-3">
          <a href="#reset" class="js-tags" style="float:right"><span class="chip">reset</span></a>
        </div>
      </div>
    </div>
  </div>
</article>
{% endif %}
{% endfor %}
{% endfor %}

<script>

var tags = document.querySelectorAll(".js-tags");

var cards = document.querySelectorAll(".js-cards");

var cardsMap = [...Array(cards.length)].map((_, i) => i);
var tagsMap = [...Array(tags.length)].map((_, i) => i)

var hashes = [];

for (var i = 0; i < tags.length; i++) {

  var tagName = tags[i].innerText;
  if (hashes.indexOf(tagName) < 0) hashes.push(tagName)

  tags[i].addEventListener("click", function() {
    var tag = this.innerText;
    tagToggle(tag)
  })
}

function tagToggle(e) {
  if (e == "reset" || hashes.indexOf(e) < 1) {
    cardsMap.map((c, index) => {
      cards[index].style.display = "block"
    });
    return
  }else{
    cardsMap.map((c, index) => {
      cards[index].style.display = "none"
    });
    tagsMap.map((t, index) => {
      if (tags[index].innerText === e) tags[index].closest(".js-cards").style.display = "block"
    })
  }
}

window.addEventListener("hashchange", function(e) {
  var tag = e.newURL.split("#")[1];
  tagToggle(tag)
})

for (var i=0; i < cards.length; i++) {
  cards[i].addEventListener("click", function(e) {
    if (!e.target.classList.contains("js-tags") && !e.target.classList.contains("chip") ) window.location = this.children[0].dataset.link
  })
}

window.onload = function() {
  var hash = window.location.hash.replace("#", '')
  tagToggle(hash)
}

</script>
