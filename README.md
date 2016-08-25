# PHP Slim Pagination
Example pagination for (slimphp/Slim)[slimphp/Slim]. No extra classes needed.

In this exmaple I used the (slimphp/Slim)[PHP Slim] framework with Twig view templates. As fronted framework ()[Semantic-UI] is used.
The shown scenarion is listing Blog posts.

## Route (index.php)
```php
$app->get('/posts', function(Request $req,  Response $res, $args = []) use ($cache) {

    $page      = ($req->getParam('page', 0) > 0) ? $req->getParam('page') : 1;
    $limit     = 5; // Number of posts on one page
    $skip      = ($page - 1) * $limit;
    $count     = Post::getCount([]); // Count of all available posts

    return $this->view->render($res, 'post-list.twig', [
        'pagination'    => [
            'needed'        => $count > $limit,
            'count'         => $count,
            'page'          => $page,
            'lastpage'      => (ceil($count / $limit) == 0 ? 1 : ceil($count / $limit)),
            'limit'         => $limit,
        ],
        // return list of Posts with Limit and Skip arguments
        'posts'         => Post::getList([
            'limit'         => $limit,
            'skip'          => $skip,
        ])
    ]);
});
```

## Template View (Twig used)
```twig
{% if pagination.needed %}
    <div class="ui pagination menu">
        {% for i in 1..pagination.lastpage %}
            <a class="{% if i == pagination.page %}active{% endif %} item" href="?page={{ i }}">{{ i }}</a>
        {% endfor %}
    </div>
{% endif %}

<div class="ui container">
    {% for post in posts %}
        <a class="item">
        	{# Post contents (title, url, ...) #}
        </a>
    {% endfor %}
</div>
```
