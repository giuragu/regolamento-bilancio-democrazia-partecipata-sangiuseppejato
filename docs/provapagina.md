# Prova pagina

## Prova

### prova la pagina

per vedere come viene>

??? quote "`provapagina2.md`"

    ``` diff 
       uno
       due tre 
       quattro
       cinque
    ```


=== "7.x"

    ``` yaml
    extra:
      version:
        provider: mike
    ```


=== "6.x"

    ``` yaml
    extra:
      version:
        method: mike
    ```


??? quote "`provapagina2.md`"

    ``` diff
@@ -61,7 +61,7 @@
            font.text | replace(' ', '+') + ':300,400,400i,700%7C' +
            font.code | replace(' ', '+')
          }}&display=fallback">
-        <style>body,input{font-family:"{{ font.text }}",-apple-system,BlinkMacSystemFont,Helvetica,Arial,sans-serif}code,kbd,pre{font-family:"{{ font.code }}",SFMono-Regular,Consolas,Menlo,monospace}</style>
+        <style>:root{--md-text-font-family:"{{ font.text }}";--md-code-font-family:"{{ font.code }}"}</style>
      {% endif %}
    {% endblock %}
    {% if config.extra.manifest %}
@@ -131,7 +131,7 @@
              {% if page and page.meta and page.meta.hide %}
                {% set hidden = "hidden" if "navigation" in page.meta.hide %}
              {% endif %}
-              <div class="md-sidebar md-sidebar--primary" data-md-component="navigation" {{ hidden }}>
+              <div class="md-sidebar md-sidebar--primary" data-md-component="sidebar" data-md-type="navigation" {{ hidden }}>
                <div class="md-sidebar__scrollwrap">
                  <div class="md-sidebar__inner">
                    {% include "partials/nav.html" %}
@@ -143,7 +143,7 @@
              {% if page and page.meta and page.meta.hide %}
                {% set hidden = "hidden" if "toc" in page.meta.hide %}
              {% endif %}
-              <div class="md-sidebar md-sidebar--secondary" data-md-component="toc" {{ hidden }}>
+              <div class="md-sidebar md-sidebar--secondary" data-md-component="sidebar" data-md-type="toc" {{ hidden }}>
                <div class="md-sidebar__scrollwrap">
                  <div class="md-sidebar__inner">
                    {% include "partials/toc.html" %}
@@ -152,7 +152,7 @@
              </div>
            {% endif %}
          {% endblock %}
-          <div class="md-content">
+          <div class="md-content" data-md-component="content">
            <article class="md-content__inner md-typeset">
              {% block content %}
                {% if page.edit_url %}
@@ -183,10 +183,18 @@
        {% include "partials/footer.html" %}
      {% endblock %}
    </div>
-    {% block scripts %}
-      <script src="{{ 'assets/javascripts/vendor.18f0862e.min.js' | url }}"></script>
-      <script src="{{ 'assets/javascripts/bundle.994580cf.min.js' | url }}"></script>
-      {%- set translations = {} -%}
+    <div class="md-dialog" data-md-component="dialog">
+      <div class="md-dialog__inner md-typeset"></div>
+    </div>
+    {% block config %}
+      {%- set app = {
+        "base": base_url,
+        "features": features,
+        "translations": {},
+        "search": "assets/javascripts/workers/search.217ffd95.min.js" | url,
+        "version": config.extra.version or None
+      } -%}
+      {%- set translations = app.translations -%}
      {%- for key in [
        "clipboard.copy",
        "clipboard.copied",
@@ -204,19 +212,12 @@
      ] -%}
        {%- set _ = translations.update({ key: lang.t(key) }) -%}
      {%- endfor -%}
-      <script id="__lang" type="application/json">
-        {{- translations | tojson -}}
-      </script>
-      {% block config %}{% endblock %}
-      <script>
-        app = initialize({
-          base: "{{ base_url }}",
-          features: {{ features or [] | tojson }},
-          search: Object.assign({
-            worker: "{{ 'assets/javascripts/worker/search.9c0e82ba.min.js' | url }}"
-          }, typeof search !== "undefined" && search)
-        })
+      <script id="__config" type="application/json">
+        {{- app | tojson -}}
      </script>
+    {% endblock %}
+    {% block scripts %}
+      <script src="{{ 'assets/javascripts/bundle.926459b3.min.js' | url }}"></script>
      {% for path in config["extra_javascript"] %}
        <script src="{{ path | url }}"></script>
      {% endfor %}
    ```
