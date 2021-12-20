# Integrate Cusdis into Hugo

[Hugo](https://docsify.js.org) is one of the most popular open-source static site generators. With its amazing speed and flexibility, Hugo makes building websites fun again.

## Usage
Here's the tutorial for integrating Cusdis into Hugo you need to find out for your template where the correct place is to include the partial.


## The template

```html

{{- if .Site.Params.cusdis.appId }}
<script>
    function callback(mutationList, observer) {
        mutationList.forEach(function(mutation) {
            if (mutation.type === 'attributes' && mutation.attributeName === 'class') {
                document.getElementById("cusdis_thread").setAttribute('data-theme', localStorage.getItem("pref-theme"));
            }
        })
    }

    const options = {
      attributes: true
    }

    const observer = new MutationObserver(callback)
    observer.observe(document.body, options)
</script>
<div id="cusdis_thread"
  data-host="{{ .Site.Params.cusdis.host | default "https://cusdis.com" }}"
  data-app-id="{{ .Site.Params.cusdis.appId }}"
  data-page-id="{{ .File.UniqueID }}"
  data-page-url="{{ .Permalink }}"
  data-page-title="{{ .Title }}"
  data-theme="{{ .Site.Params.defaultTheme }}"
></div>
{{- if ne .Site.Language.Lang "en" }}{{/* English is the default so we do not need to load the language file */}}
<script defer src="https://cusdis.com/js/widget/lang/{{ .Site.Language.Lang }}.js"></script>
{{- end }}
<script async defer src="https://cusdis.com/js/cusdis.es.js"></script>
{{- end }}
```

This template can be placed in a theme partial and can be configured with the following settings from the `config.yml` or `config.toml`.

```yml
params:
  cusdis:
    host: "" # Leaving this empty will use the default host
    appId: xxxxxxxx # Without filling in an appId the script will not be added
```

The javascript in the template is used for toggling the dark and the light theme. This does currently not work because the values are only used at startup but there is an [issue active #170](https://github.com/djyde/cusdis/issues/170) it currently checks the body element of the page if there is an class added and than checks the local storage for the `pref-theme` key to add the value `light` or `dark` to the Cusdis `data-theme` attribute.


