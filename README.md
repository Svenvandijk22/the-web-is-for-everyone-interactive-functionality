# Interactive Functionality

# Milledoni

Milledoni is een online cadeau-inspiratieplatform waar gebruikers op een eenvoudige manier originele en passende cadeaus kunnen vinden. De website werkt met een community van cadeauspotters die cadeau-ideeën verzamelen en delen. Gebruikers kunnen filteren op verschillende kenmerken zoals leeftijd, budget en gelegenheid. De producten worden niet direct verkocht op de website, maar via externe webshops aangeboden.

## Inhoudsopgave

  * [Beschrijving](#beschrijving)
  * [Gebruik](#gebruik)
  * [Kenmerken](#kenmerken)
  * [Installatie](#installatie)
  * [Bronnen](#bronnen)
  * [Licentie](#licentie)

## Beschrijving

Tijdens deze sprint heb ik mij gefocust op drie belangrijke onderdelen binnen het project: het toevoegen van post-interacties, het verbeteren van de responsive layout en het ontwikkelen van een filter- en sorteerfunctie. Voor de post-interacties heb ik een functionaliteit gebouwd waarmee gebruikers producten kunnen opslaan in een persoonlijke lijst op de liked products pagina. Daarnaast heb ik ook een maniertoegevoegd om deze opgeslagen producten weer te verwijderen. De uitwerking van deze interacties en de  flow heb ik vooraf visueel gemaakt in een wireflow, die te vinden is via deze issue: https://github.com/Svenvandijk22/the-web-is-for-everyone-interactive-functionality/issues/1


Producten opslaan

Gebruikers kunnen producten opslaan in een persoonlijke lijst. Deze worden weergegeven op de index pagina pagina.
```
app.post("/opslaan", async function (request, response) {
  await fetch(
    "https://fdnd-agency.directus.app/items/milledoni_users_milledoni_products_1",
    {
      method: "POST",
      body: JSON.stringify({
        milledoni_users_id: 63,
        milledoni_products_id: request.body.id,
      }),
      headers: {
        "Content-Type": "application/json;charset=UTF-8",
      },
    },
  );

  response.redirect(303, "/likedproducts");
});

```


Producten verwijderen

Gebruikers kunnen opgeslagen producten ook weer verwijderen.

```

app.post("/verwijder", async function (request, response) {
  const linkIDresponse = await fetch(`https://fdnd-agency.directus.app/items/milledoni_users_milledoni_products_1?filter[milledoni_users_id][_eq]=63&filter[milledoni_products_id][_eq]=${request.body.id}&fields=id&limit=1`)
  const linkIDjson = await linkIDresponse.json();
  const linkID = linkIDjson.data[0].id;

  await fetch(
    'https://fdnd-agency.directus.app/items/milledoni_users_milledoni_products_1/' + linkID,
    {
      method: "DELETE",
      headers: {
        "Content-Type": "application/json;charset=UTF-8",
      }
    }
  );

  response.redirect(303, "/likedproducts");
});

```


Naast de interactieve functionaliteiten heb ik ook gewerkt aan het verbeteren van de responsive layout van de website. Hierbij heb ik ervoor gezorgd dat de interface goed werkt op desktop, tablet en mobiel. Op desktop is de layout verdeeld in twee kolommen met links de chatbot en rechts de producten, terwijl op tablet en mobiel deze onderdelen onder elkaar worden weergegeven. Het verschil tussen tablet en mobiel zit voornamelijk in de manier waarop producten worden gepresenteerd: op tablet worden producten in twee kolommen getoond, vergelijkbaar met desktop, terwijl op mobiel één enkele kolom wordt gebruikt. De uitwerking van de responsive aanpassingen is terug te vinden in deze issue: https://github.com/Svenvandijk22/the-web-is-for-everyone-interactive-functionality/issues/10


Daarnaast heb ik de website getest in verschillende browsers om te controleren of de layout en functionaliteiten consistent werken. Deze tests en bevindingen zijn gedocumenteerd in de volgende issue: https://github.com/Svenvandijk22/the-web-is-for-everyone-interactive-functionality/issues/8


Tot slot heb ik een filter- en sorteersysteem toegevoegd als aanvulling op de bestaande chatbotfunctionaliteit. Gebruikers kunnen via een filterknop een overzicht openen waarin zij producten kunnen sorteren, bijvoorbeeld op prijs van laag naar hoog of van hoog naar laag. Deze functionaliteit maakt het mogelijk om ook binnen gefilterde resultaten verder te sorteren. De uitwerking hiervan is te vinden in deze issue: https://github.com/Svenvandijk22/the-web-is-for-everyone-interactive-functionality/issues/6

<!-- Bij Beschrijving staat kort beschreven wat voor project het is en wat je hebt gemaakt -->
<!-- Voeg een mooie poster visual of video toe 📸 -->
<!-- Voeg een link toe naar GitHub Pages 🌐-->

## Gebruik
<!-- Bij Gebruik staat de user story, hoe het werkt en wat je er mee kan. -->

## Kenmerken
om ingeladen data weer te geven heb ik gebruik gemaakt van deze loops in me liquid
```
 {% for product in products %}
        <div class="producten-kaart">
          <img class="strikje" src="/assets/strikje.svg" alt="strikje">
         
          <img src="{{ product.image }}" alt="product">
          <h2>{{ product.name }}</h2>
         

          <form method="post" action="opslaan" class="like-form">
       
            <button type="submit" class="lijst-button" value="{{ product.id }}" name="id" >
              <img class="lijstje" src="/assets/lijstje.png" alt="lijst">
            </button>
          </form>
        </div>
      {% endfor %}

```


loop die ik gebruik voor mijn products pagina: 
```
  {% for product in likedProducts %}
    <article class="opgeslagen-product">
       <img class="strikje-twee" src="/assets/strikje.svg" alt="strikje">
      
      <img src="{{ product.milledoni_products_id.image }}" alt="{{ product.milledoni_products_id.name }}">
      <h2>{{ product.milledoni_products_id.name }}</h2>
      <p>{{ product.milledoni_products_id.amount }}</p>
        <form method="post" action="verwijder" class="like-form">
       
            <button type="submit" class="lijst-button" value="{{ product.milledoni_products_id.id }}" name="id" >
              <img class="lijstjemin" src="/assets/minnetje.png" alt="lijst">
            </button>
          </form>
    </article>
  {% endfor %}
```

## Installatie
1 npm install in de terminal Hiermee installeer je de benodigde packages .

2 npm start in de terminal Hiermee start je het project.

3 Open vervolgens http://localhost:8000 om de website te zien in de browser


## Bronnen

## Licentie

This project is licensed under the terms of the [MIT license](./LICENSE).
