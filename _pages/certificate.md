---
title: "Certificate"
permalink: /certificate/
---

{% include group-by-array.html collection=site.gallery field='tags' %}

<ul>
  {% for tag in group_names %}
    {% assign posts = group_items[forloop.index0] %}

    <li>
      <h2>{{ tag }}</h2>
      <ul>
        {% for post in posts %}
        <li>
          <a href='{{ site.baseurl }}{{ post.url }}'>{{ post.title }}</a>
        </li>
        {% endfor %}
      </ul>
    </li>
  {% endfor %}
</ul>




[comment]: <> ({% include figure image_path="/assets/images/certificates/Jian Jian_eDiploma.jpg" alt="" %})

[comment]: <> ({% include figure image_path="/assets/images/certificates/Coursera WR4TAH5S5XZV.jpg" alt="" %})

[comment]: <> ({% include figure image_path="/assets/images/certificates/CertificateOfCompletion_C Essential Training 2018.jpg" alt="" %})

[comment]: <> ({% include figure image_path="/assets/images/certificates/Data science-Stanford.jpg" alt="" %})

[comment]: <> ({% include figure image_path="/assets/images/certificates/Coursera FZEY8VFN9SSB.jpg" alt="" %})

[comment]: <> ({% include figure image_path="/assets/images/certificates/GDPR.jpg" alt="" %})

[comment]: <> ({% include figure image_path="/assets/images/certificates/Coursera SP87NNXXRUVT.jpg" alt="" %})

[comment]: <> ({% include figure image_path="/assets/images/certificates/M001_proof_of_completion.jpeg" alt="" %})

[comment]: <> ({% include figure image_path="/assets/images/certificates/SCJP-certification.jpg" alt="" %})