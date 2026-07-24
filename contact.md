---
layout: page
title: evo Bond Legacy | Montana Park | Pretoria
background: grey
---
<div class="col-lg-12 text-center">
  <h1 class="section__heading text-uppercase">Contact {{ site.company }} </h1>
</div>

<br>

<div class="container contact-us">
  <div class="row">
    
    <!-- Left Card: Office Hours -->
    <div class="col-md-6">
      <div class="contact-card office-hours-card">
        <div class="contact-card__header">
          <i class="fas fa-clock contact-card__icon"></i>
          <h3 class="contact-card__title">Office Hours</h3>
        </div>
        <div class="contact-card__body">
          <div class="hours-row">
            <span class="hours-day">Monday - Friday</span>
            <span class="hours-time">8:00 AM - 5:00 PM</span>
          </div>
          <div class="hours-row">
            <span class="hours-day">Saturday</span>
            <span class="hours-time">9:00 AM - 1:00 PM</span>
          </div>
          <div class="hours-row closed">
            <span class="hours-day">Sunday & Public Holidays</span>
            <span class="hours-time">Closed</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Right Card: Contact Person -->
    <div class="col-md-6">
      <div class="contact-card person-card">
        <div class="contact-card__body">
          <h3 class="contact-card__name">{{ site.data.sitetext.team.people[0].name }}</h3>
          <p class="contact-card__role">{{ site.data.sitetext.team.people[0].role }}</p>
          <div class="contact-card__links">
            <a href="mailto:{{ site.contactmail | default: site.email }}?subject=evo Bond Legacy Website Enquiry" class="contact-link" aria-label="Email {{ site.data.sitetext.team.people[0].name }}">
              <i class="fas fa-envelope"></i>
              <span>Email</span>
            </a>
            <a href="tel:+{{ site.contactwhatapp }}" class="contact-link" aria-label="Call {{ site.data.sitetext.team.people[0].name }}">
              <i class="fas fa-phone"></i>
              <span>Call</span>
            </a>
            <a href="https://api.whatsapp.com/send?phone={{ site.contactwhatapp }}" class="contact-link" aria-label="WhatsApp {{ site.data.sitetext.team.people[0].name }}" target="_blank" rel="noopener">
              <i class="fab fa-whatsapp"></i>
              <span>WhatsApp</span>
            </a>
          </div>
        </div>
      </div>
    </div>

  </div>
</div>
