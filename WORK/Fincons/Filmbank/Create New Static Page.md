---
tags:
  - Fincons/Filmbank/WordPress
---

# Create New Static Page

- [ ] Create new template (Because every page in WordPress can have a template that explain how should be populated and styled). Than again crete a `template-<new template name>.php` (see template-terms)
- [ ] _Note_ the comment on top assign the name,

```
/*
Template Name: Accessibility Statement
*/
```

---

- [ ] _NOTE_ the following `while loop` generate a HTML content taking that from WordPress dashboard under page section, from pages with Accessibility Statement template

```php
<?php
	while ( have_posts() ) :
		the_post();
		the_content();
	endwhile;
?>
```

---

- [ ] Move to WordPress dashboard create a new Page naming as you want, just remember that the title could be insert by the following script and the URL is generated from title just insert

```php
 <h1 class="page-title"><?php the_title(); ?></h1>
```

---

- [ ] Go to `wp-staticize.php` (this file is particular important because generates the actual `index.html` on AWS S3 bucket for every static page)

  - [ ] [S3 - dds-test-static-pages](https://eu-west-1.console.aws.amazon.com/s3/buckets/dds-test-static-pages?region=eu-west-1&bucketType=general&tab=objects)
  - [ ] [S3 dds-prod-static-pages](https://eu-west-1.console.aws.amazon.com/s3/buckets/dds-prod-static-pages?region=eu-west-1&bucketType=general&tab=objects)

- [ ] Check this out - Simple static pages like `terms-conditions/` has a folder holding the `.html` file and a in the root there is a `terms-conditions.json` holding the WordPress content divided in posts. This content is used to generate html from the previous `while loop`

- [ ] To update S3 you must add (should check out those commits on develop `237d3ba`, `0b333cc` for more)

- [ ] for `.json` file generation to `wp-staticize.php`

```php
if(!$skipAccessibility) update_json_accessibility($static_pages_min_period, $static_pages_save_clean_fs);

function update_json_accessibility($delta_max, $static_pages_save_clean_fs){ ... }
```

---

- [ ] add all skip flags for `.html` file generation to `wp-staticize.php`

```
$skipAccessibility = false;
$skipAccessibility = true;
....
```

---

- [ ] S3 update take some time, remember to regen pages on Admin-portal after WordPress update

---
