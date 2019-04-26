---
layout: main
---

<main class="home" id="post" role="main" itemprop="mainContentOfPage" itemscope="itemscope" itemtype="http://schema.org/Blog">
    <div id="grid" class="row flex-grid">
    {% for post in site.posts %}
		<!-- 文章 -->
        <article class="box-item" itemscope="itemscope" itemtype="http://schema.org/BlogPosting" itemprop="blogPost">
            <span class="category">
                <a href="{{ site.url }}{{ site.baseurl }}/category/{{ post.category }}">
                    <span>{{ post.category }}</span>
                </a>
            </span>
            <div class="box-body">
                {% if post.image %}
					<!-- 图片 -->
                    <div class="cover">
						<!--加载判断 是否是最近一周发布-->
                        {% include new-post-tag.html date=post.date %}
                        <a href="{{ post.url | prepend: site.baseurl }}" {%if isnewpost %}class="new-post"{% endif %}>
                            <img src="assets/img/placeholder.png" data-url="{{ post.image }}" class="preload">
                        </a>
                    </div>
                {% endif %}
                <div class="box-info">
                    <meta itemprop="datePublished" content="{{ post.date | date_to_xmlschema }}">
                    <!-- 日期 -->
					<time itemprop="datePublished" datetime="{{ post.date | date_to_xmlschema }}" class="date">
                        {% include date.html date=post.date %}
                    </time>
					<a class="post-link" href="{{ post.url | prepend: site.baseurl }}">
                        <!-- 标题 -->
						<h2 class="post-title" itemprop="name">
                            {{ post.title }}
                        </h2>
                    </a>
                    <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">
                        <!-- 描述 -->
						<p class="description">{{ post.introduction }}</p>
                    </a>
					<!-- 标签 -->
                    <div class="tags">
                        {% for tag in post.tags %}
                            <a href="{{ site.baseurl}}/tags/#{{tag | slugify }}">{{ tag }}</a>
                        {% endfor %}
                    </div>
                </div>
            </div>
        </article>
    {% endfor %}
    </div>
</main>
