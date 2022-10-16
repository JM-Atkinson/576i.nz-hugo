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
  margin: 0 6rem;
}

.card {
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-around;
  text-align: right;
  max-width: 174.217px;
  min-width: 174.217px;
  padding: 1rem 0;
}

@media (width < 400px) {
  #cards {
    margin: unset;
  }
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

.card-text .description {
  font-size: 0.75rem;
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

    const createWebsiteCard = (domain) => {

        const card = document.createElement('a');

        card.setAttribute('href', `https://${domain}`);
        card.setAttribute('target', '_blank');
        card.setAttribute('rel', `noreferrer`);
        card.setAttribute('class', 'card');

        card.innerHTML = 
        `<img src="https://icon.horse/icon/${domain}" alt="${domain}" class="card-image">
        <div class="card-text">
            <h6 class="title"><b>${domain}</b></h6>
        </div>`

        return card;
    }

    const renderWebsiteCards = () => {

        const domains = ['binu.nz','manoj.nz','matteas.nz','pancake.nz','timo.nz']

        domains.forEach(domain => {
            const websiteCard = createWebsiteCard(domain);
            document.getElementById("cards").appendChild(websiteCard);
        })
        
    }

    renderWebsiteCards();

</script>
