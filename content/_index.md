---
title:
---

<img id="logo" src="576i.svg"></img>
<style>
#logo {
  margin-bottom: 60px;
}

a, a:visited, a:hover, a:active {
  color: black;
  text-decoration: none;
}

#cards {
  display: flex;
  flex-flow: wrap;
  gap: 1rem;
  justify-content: center;
}

.card {
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
  display: flex;
  flex-basis: 31%;
  text-align: right;
  padding: 1rem 1rem 0 1rem;
}

@media (width < 600px) {
  .card {
    flex-grow: 1;
    flex-basis: 48%;
  }
}

.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
}

.card-image {
  width: 20%;
}

.container {
  padding: 2px 16px;
}
</style>

Welcome to my wax lyrical about technology, product management, and other topics.

Are you looking for articles? You should check out [Articles](/post). If you want to learn about me, check out [About](/about) or visit my [LinkedIn](https://www.linkedin.com/in/josh-atkinson/).

#### Friends of 576i.nz
<div id="cards">
</div>

<script>

    const createWebsiteCard = (domain, description) => {

        const card = document.createElement('a');

        card.setAttribute('href', `https://${domain}`);
        card.setAttribute('target', '_blank');
        card.setAttribute('rel', `noreferrer`);
        card.setAttribute('class', 'card');

        card.innerHTML = 
        `<img src="https://icon.horse/icon/${domain}" alt="${domain}" class="card-image">
        <div class="description">
            <h4><b>${domain}</b></h4> 
            <p>${description}</p> 
        </div>`

        return card;
    }

    const renderWebsiteCards = () => {

        const cards = [
            {
                'domain': 'binu.nz',
                'description': 'Lorem ipsum dolor sit amet'
            },
            {
                'domain': 'manoj.nz',
                'description': 'Lorem ipsum dolor sit amet'
            },
            {
                'domain': 'matteas.nz',
                'description': 'Lorem ipsum dolor sit amet'
            },
            {
                'domain': 'pancake.nz',
                'description': 'Lorem ipsum dolor sit amet'
            },
            {
                'domain': 'timo.nz',
                'description': 'Lorem ipsum dolor sit amet'
            },
        ]

        cards.forEach(card => {
            const websiteCard = createWebsiteCard(card.domain, card.description);
            document.getElementById("cards").appendChild(websiteCard);
        })
        
    }

    renderWebsiteCards();

</script>
