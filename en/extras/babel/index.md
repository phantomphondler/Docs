---
title: "Babel"
_old_id: "605"
_old_uri: "revo/babel"
---

## What is Babel?

Babel is an Extra for MODX Revolution that helps you managing your multilingual websites using different contexts. Babel even supports managing several different multilingual websites within one MODX instance by using so called context groups.

Babel maintains links between translated resources. In the manager you can use the Babel Box to easily switch between the different language versions of your resources. Translations can be created automatically by Babel or defined manually.

Additionally Babel can be used to synchronize certain template variables (TVs) of translated resources which should be the same in every context (language).

For detailed information about Babel check out the [Offical Babel project website: Multilingual websites with MODX](http://www.multilingual-modx.com/).

## History

Babel has been developed by Jakob Class based on ideas of Sylvain Aerni and first released on December 16th, 2010.

## Development and Bug Reporting

Babel is stored and developed in GitHub, and can be found here: <https://github.com/mikrobi/babel>

Bugs can be filed here: <https://github.com/mikrobi/babel/issues>

## Installation

### Preparations

Create a context for each language and set the cultureKey and site\_url settings according to your needs. You may have a look at our [tutorial to setup your multilingual site(s)](http://www.multilingual-modx.com/blog/2011/multilingual-websites-with-modx-and-babel.html).

Be sure that your context switches work well.

### Download

babel can be downloaded from within the MODX Revolution manager via [Package Management](building-sites/extras "Package Management"), or from the MODX Extras Repository, here: <https://modx.com/extras/package/babel>

### Setup

Install Babel via the package manager and set the system settings for Babel via the form displayed during setup:

![](babel-setup.png)

- **Context Keys** (_babel.contextKeys_): Comma separated list of context keys which should be used to link multilingual resources.
  For advanced configuration you may define several groups of context keys by using a semicolon (;) as delimiter. This is usefull if your're administrating multiple multilingual sites within one MODX instance.
  Example scenario:
    - **site1**: en, de, fr. using contexts: _web, site1de, site1fr_
    - **site2**: en, de. using contexts: _site2en, site2de_

You would set _babel.contextKeys_ to "_web_**_,_**_site1de_**_,_**_site1fr_**_;_**_site2en_**_,_**_site2de_".

- **Name of Babel TV** (_babel.babelTvName_): Name of template variable (TV) in which Babel will store the links between multilingual resources. This TV will be maintained by Babel. Please don't change this TV manually otherwhise your translation links may be broken.
- **IDs of TVs to be synchronized** (_babel.syncTvs_): Comma separated list of ids of template variables (TVs) which should be synchronized by Babel.

## How to Use

When you open a resource for editing, the Babel Box will be displayed on top of the resource form. There will be button-like links for each language (context) you have defined in the babel.contextKeys system setting.

![](babel.png)

The buttons may have three different colors according to their state:

- **Black**: Language of the currently displayed resource.
- **Green**: Language for which a translated resource is defined.
- **Light Gray**: Language for which no translation has been created or defined yet.

By clicking on the (green) language buttons you can easily switch between the different language versions of your resources.

If there are no translations defined for certain language (gray button), mousover the language's button: a layer appears where you can tell Babel to create a translation of the current resource or you can set the translation link to an existing resource manually by entering the ID of the translated resource.

![](babel-translate.png)

When clicking on "Create Translation" Babel will create a new resource in the language's context and copy all the content of the current resource to the newly created resource. You now can translated all the content and TVs and publish the translated resource.

If you'd like to remove a translation link, just mouseover the (green) language button: a layer appears where you can click on "Unlink translation" button to remove the translation link to this language.

![](babel-unlink.png)

### Snippets

Currently there are two snippets available for Babel: [BabelLinks](extras/babel/babellinks "Babel.BabelLinks") and [BabelTranslation](extras/babel/babeltranslation "Babel.BabelTranslation").

## Change Babel Settings after Installation

You may change your Babel settings after setup. For example if you want to define a new TV which should be synchronized or add a new context. For doing this go to System/Settings in your MODX manager and select the babel namespace. Now you can edit all Babel related settings:

![](babel-settings.png)

### Tutorial

As http://www.multilingual-modx.com/blog/2011/multilingual-websites-with-modx-and-babel.html does not exist / is offline see below the copy pasted from  this website:

#### Step by Step Instructions

Use a new clean MODx environment you should be ready to setup your multilingual website.

I’ll go through all the necessary steps by configuring an example website which will be available in three languages: English, German and French. The English site will be available via www.example.com the German site via www.exmaple.de and the French one via www.example.fr.

    Create your contexts for each language:
    The default web context will be used for the English site, a de context for the German and a fr for the French site:
    create a context for each language
    Configure language specific settings of all your contexts: site_url and cultureKey.
        web context: site_url: http://www.example.com, cultureKey: en
        de context: site_url: http://www.example.de, cultureKey: de
        fr context: site_url: http://www.example.fr cultureKey: fr
    create language specific settings for each context
    Grant the "Load Only" access policy for all your contexts to the anonymous group to let your users load the contexts:
    grant access to contexts
    Create a gateway plugin which listens to the "OnHandleRequest" event to load the context and set the correct cultureKey:

    <?php
    if($modx->context->get('key') != "mgr"){
    	//grab the current domain from the http_host option
    	switch ($modx->getOption('http_host')) {
    		case 'www.example.de':
    			//switch the context
    			$modx->switchContext('de');
    			//set the cultureKey
    			$modx->setOption('cultureKey', 'de');
    			break;
    		case 'www.example.fr':
    			$modx->switchContext('fr');
    			$modx->setOption('cultureKey', 'fr');
    			break;
    		default:
    			// Set the default language/context here
    			$modx->switchContext('web');
    			$modx->setOption('cultureKey', 'en');
    			break;
    	}
    }

    Install the Babel Extra via package management.

Now you should be able to create documents and link translations via Babel in your MODx manager. For information about how to use the MODx lexicon feauture you may take a look at the tutorial at digital butter or official documentation.
Common Mistakes

In order to avoid mistakes during setting up your contexts here are some common mistakes which have been made by users who requested my help:
Babel does not display the correct language of a context

You may have forgotten to configure the cultureKey setting in the specific context. Babel uses this setting to determine the language of a context.
Babel does not display all links to translated documents in the front-end

You may have forgotten to grant the "Load Only" access policy for your visitors to the specific contexts. Or you may have forgotten to flush the permissions.
The site is always displayed in the same language

Check your gateway plugin and make sure it works properly. When using the code from above be sure that the http_host setting is not set in your context settings. Otherwise your gateway plugin won’t be able to determine the requestested URL.

###SEO Friendly Multilingual Websites with MODx and Babel

In my previous article about setting up multilingual websites with MODx and Babel I described a solution which is based on different (sub)domains for each language. This domain based approach is implemented easily but has some drawbacks in a SEO point of view: By using different domains for each language you automatically split up your site into several single sites. Each site will be handled separately by search engines. For example they won't share the same page rank and backlinks. Using one domain and subfolders for each language may improve your site's overall ranking: All backlinks are connected to your top level domain. In this article I'll describe a possible solution of how to setup a multilingual website with MODx and Babel by using one domain and subfolders for each language.

This article doesn't focus on the SEO point of view. It's rather a technical tutorial of how to setup a multilingual website by using subfolders. If you'd like to read more about the "(sub)domains vs. subfolders" topic you may search the web (there are a lot of articles about this topic) or read some of the following posts of other blogs:

    Subfolders v/s Subdomains: Which one to choose for SEO?
    Subdomains or Subfolders : Which are Better for SEO?
    Subdomains, Subfolders and Top-Level Domains
    Subdomains and subdirectories

Technical Background

I'll describe the procedure of setting up the a multilingual site by providing a fictional example site http://www.example.com. The main website is reachable via http://www.example.com/ and is available in two languages:

    German: http://www.example.com/de/
    English: http://www.example.com/en/

For each language we are using one context: web for German, en for English. To determine the proper culture key we are using some rewrite rules in the .htaccess file. These rules check for the first subfolder of the requested URL and set the cultureKey request parameter which is used by MODx to initialize the lexicon.
Prerequisites

Before starting with this tutorial you should be sure that all requirements for a multilingual site are satisfied:

    Friendly URLs are enabled: friendly_urls and use_alias_path are set to yes (1)
    The Apache rewrite engine is activated and the rewrite base is set correctly:

    RewriteEngine On
    RewriteBase /

    If you're running your site in a non-root directory like /subfolder/mysite/xy you have to define your rewrite base like this:

    RewriteBase /subfolder/mysite/xy/

    The base URL is set via the <base> Tag in your HTML head of all your templates:

    <head>
    	...
    	<base href="[[++site_url]]" />
    	...
    </head>

Step by Step Instructions

You have to follow the five steps described in my previous article about setting up multilingual websites and one additional step:

    Create your contexts for each language: no differences to domain based approach.
    Configure language specific settings of all your contexts: site_url, cultureKey and base_url.
    Differences: Instead of using different domains for the site_url setting you have to use subfolders and additionally specifiy the base_url according to the context's cultureKey:
    web context: site_url: http://www.example.com/de/ base_url: /de/
    en context: site_url: http://www.example.com/en/ base_url: /en/
    Hint: You should also define settings like site_start (id of default landing page), error_page, etc. for each of your contexts.
    Grant the "Load Only" access policy for all your contexts to the anonymous group: no differences to domain based approach.
    Create a gateway plugin which listens to the "OnHandleRequest" event to load the correct context.
    Differences: Instead of using the requested domain to determine the context, the cultureKey request parameter is used which is set by some rewrite rules (see below). Additionally there is no need to set the cultureKey of the modx object anymore:

    <?php
    if($modx->context->get('key') != "mgr"){
    	/* grab the current langauge from the cultureKey request var */
    	switch ($_REQUEST['cultureKey']) {
    		case 'en':
    			/* switch the context */
    			$modx->switchContext('en');
    			break;
    		default:
    			/* Set the default context here */
    			$modx->switchContext('web');
    			break;
    	}
    	/* unset GET var to avoid
    	 * appending cultureKey=xy to URLs by other components */
    	unset($_GET['cultureKey']);
    }

    Install the Babel Extra via package management: no differences to domain based approach.
    Change existing rewrite rules for friendly URLs and add additional rules to your .htaccess file (see next section for detailed description):

     The Friendly URLs part
     detect language when requesting the root (/)
    RewriteCond %{HTTP:Accept-Language} !^de [NC]
    RewriteRule ^$ en/ [R=301,L]
    RewriteRule ^$ de/ [R=301,L]

     redirect all requests to /en/favicon.ico and /de/favicon.ico
     to /favicon.ico
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(en|de)/favicon.ico$ favicon.ico [L,QSA]

     redirect all requests to /en/assets* and /de/assets* to /assets*
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(en|de)/assets(.*)$ assets$2 [L,QSA]

     redirect all other requests to /en/* and /de/*
     to index.php and set the cultureKey parameter
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(en|de)?/?(.*)$ index.php?cultureKey=$1&q=$2 [L,QSA]

Adding Rewrite Rules

To make your multilingual site work properly you have to add some rewrite rules to your .htaccess file which handle requests to your (physically non-existing) language subfolders.

First you have to replace the default rewrite rule shipped with the MODx ht.access file to the following:

 The Friendly URLs part
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(en|de)?/?(.*)$ index.php?cultureKey=$1&q=$2 [L,QSA]

If you're using other languages than German and English you have to change the (en|de) part of the rewrite rule according to your needs. For example for a website available in English, Spanish and French you would use the following rewrite rule:

RewriteRule ^(en|es|fr)?/?(.*)$ index.php?cultureKey=$1&q=$2 [L,QSA]

Ok, now your pages should be accessible via the language subfolders and linking your pages with relative links should work, too.

But there is still a problem regarding relative links: linking assets like CSS, JavaScripts, images etc. won't work properly. Normally all these files are located somewhere in the assets subfolder of your MODx root directory. When including an asset via a relative URL like assets/css/style.css the asset won't be found:

    The browser will try to request something like http://www.example.com/en/assets/css/style.css because the site's URL http://www.example.com/en/ (defined via the site_url context setting in step 2) is used to handle relative URLs.
    The rewrite rule from above will be applied and the request will be internally forwarded to http://www.example.com/index.php?cultureKey=en&q=assets/css/style.css
    MODx won't find any resource matching the alias assets/css/style.css and will return a 404 error code.

To solve this problem you have to add another rewrite rule before the rule from above which internally redirects all request to /[ck]/assets/* to /assets/* where [ck] is a valid culture key:

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(en|de)/assets(.*)$ assets$2 [L,QSA]

Fine! Now you can use relative links for your pages and assets. Including images with TinyMCE should work, too.

You may want to add some additional rewrite rules for other files which are being referred via relative URLs. For example the favicon.ico in your root directory:

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(en|de)/favicon.ico$ favicon.ico [L,QSA]

Additionally the server should automatically determine the language when the domain root (http://www.example.com/) is requested and perform a redirect to the suitable language version in the following way:

    When the accepted language is not German (de) and the root has been requested (relative request URI is empty) redirect to the English version (en/): see first condition and rewrite rule from below (line 2 and 3).
    Otherwise redirect to German version (de/) when the root has been requested: see second rewrite rule from below (line 4).

 detect language when requesting the root (/)
RewriteCond %{HTTP:Accept-Language} !^de [NC]
RewriteRule ^$ en/ [R=301,L]
RewriteRule ^$ de/ [R=301,L]

Hint: That's very rudimentary. The condition only checks whether the value of the Accept-Language HTTP header variable begins with the language (culture) key. But this variable contains much more than only a language key: Its a list of preferred (or even non-preferred) keys like this: Accept-Language: de-de,de;q=0.8,en-us;q=0.5,en;q=0.3. The q variable specifies the importance of the language from 0 to 1. Detecting the language with PHP in the gateway plugin is much better. But this is not the topic of this article and will be discussed in another post.

Ok now all rules and conditions can be added to your .htaccess file. It's very important to place them in the right order because Apache goes through the rules from top to bottom:

 The Friendly URLs part
 detect language when requesting the root (/)
RewriteCond %{HTTP:Accept-Language} !^de [NC]
RewriteRule ^$ en/ [R=301,L]
RewriteRule ^$ de/ [R=301,L]

 redirect all requests to /en/favicon.ico and /de/favicon.ico
 to /favicon.ico
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(en|de)/favicon.ico$ favicon.ico [L,QSA]

 redirect all requests to /en/assets* and /de/assets* to /assets*
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(en|de)/assets(.*)$ assets$2 [L,QSA]

 redirect all other requests to /en/* and /de/*
 to index.php and set the cultureKey parameter
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(en|de)?/?(.*)$ index.php?cultureKey=$1&q=$2 [L,QSA]

If you'd like to go deeper into defining rewrite rules you should read more about the Apache mod_rewrite module.
Is this approach optimal?

This solution works fine and editors can work as they did before without caring about relative links and subfolders. But I think this approach is rather a workaround than an optimal solution:

    When linking assets relatively you link to non-existing "virtual" files.
    By applying the rewrite rules for the assets the same file is served via several different URLs: http://www.example.com/assets/css/style.css, http://www.example.com/de/assets/css/style.css and http://www.example.com/en/assets/css/style.css return the same content.
    Files which are used in all language versions are not cached for the whole site by your browser: The browser doesn't know that http://www.example.com/de/assets/css/style.css and http://www.example.com/en/assets/css/style.css are the same.
    When working with the GoogleSiteMap Extra you won't be able to serve a sitemap.xml for your whole site without modifying the Extra manually. This is because your documents are distributed over several contexts and GoogleSiteMap is only capable of creating a sitemap for one context. XML sitemaps are very helpful to tell a search engine bot where to find all your pages. So you should use them and they should list all pages of your site!


## See Also

1. [Babel.BabelLinks](extras/babel/babellinks)
2. [Babel.BabelTranslation](extras/babel/babeltranslation)
