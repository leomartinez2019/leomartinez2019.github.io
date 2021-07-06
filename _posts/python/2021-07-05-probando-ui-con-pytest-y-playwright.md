---
layout: post
title: "Probando UI con Pytest y Playwright"
description: "Playwright como alternativa a Selenium"
date: 2021-07-05 22:45:00 -0500
categories: python
---

<div class="foto-center" align="center">
  <img src="{{site.baseurl}}/imagenes/test-automation.png" alt="automation site" />
  <figcaption>UI Playground</figcaption>
</div>

Playwright es una librería de automatización de navegadores Web creada y mantenida por Microsoft. Funciona con los navegadores Chromium, Firefox y Webkit. Se puede enlazar con los lenguajes de programación Python, Java, .Net y JavasCript. La extensión de Python tiene un plugin para ser usado con Pytest.

## Instalación

{% highlight bash %}
pip install playwright
playwright install  # También: python -m playwright install
{% endhighlight %}

Ejemplo de uso para visitar una página:

{% highlight python %}
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch()
    page = browser.new_page()
    page.goto("http://playwright.dev")
    print(page.title())
    browser.close()
{% endhighlight %}

Para usar con pytest instalamos el wrapper que nos permite usar fixtures:

{% highlight bash %}
pip install pytest-playwright
{% endhighlight %}

Las funciones tienen acceso a los fixtures cuando se usan como argumentos de la función:

{% highlight python %}
def test_invalid_input(page):
    page.goto("/sampleapp")
    assert page.inner_text('h3') == 'Sample App'
    page.click("text=Log In")
    assert page.inner_text('#loginstatus') == 'Invalid username/password'

def test_bad_button(page):
    page.goto("/click")
    assert page.inner_text('h3') == 'Click'
    button_class = page.get_attribute('#badButton', 'class')
    assert button_class == 'btn btn-primary'
    page.click("#badButton")
    button_class = page.get_attribute('#badButton', 'class')
    assert button_class == 'btn btn-success'
{% endhighlight %}

El archivo de configuración de pytest se puede usar para señalar la URL base:

{% highlight python %}
[pytest]
# Run firefox with UI
addopts = --base-url http://www.uitestingplayground.com/
{% endhighlight %}

Al ejecutar el test se puede crear un reporte en formato HTML. Previamente hay que instalar el paquete:

{% highlight bash %}
pip install pytest-reporter-html1
{% endhighlight %}

Y con este comando realizamos el test y creamos el reporte:

{% highlight python %}
pytest --template=html1/index.html --report=report.html
{% endhighlight %}

Y este es el reporte que se obtiene:

<div class="foto-center" align="center">
  <img src="{{site.baseurl}}/imagenes/test-report-pytest.png" alt="test report" />
  <figcaption>Test Report</figcaption>
</div>