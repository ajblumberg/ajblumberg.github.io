---
layout: page
permalink: /publications/
title: papers
description:
nav: true
nav_order: 2
---

<style>
.pub-filter-tabs {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
  margin-bottom: 1.5rem;
}
.pub-filter-tabs button {
  padding: 0.35rem 0.9rem;
  border: 1px solid var(--global-theme-color, #23373b);
  background: transparent;
  color: var(--global-theme-color, #23373b);
  border-radius: 3px;
  font-size: 0.88rem;
  cursor: pointer;
  transition: background 0.15s, color 0.15s;
}
.pub-filter-tabs button.active,
.pub-filter-tabs button:hover {
  background: var(--global-theme-color, #23373b);
  color: #fff;
}
</style>

<div class="pub-filter-tabs">
  <button class="active" data-filter="all">all</button>
  <button data-filter="math">math</button>
  <button data-filter="cs">CS</button>
  <button data-filter="gda">geometric data analysis</button>
  <button data-filter="compbio">computational biology</button>
</div>

<!-- _pages/publications.md -->
<div class="publications">

{% bibliography %}

</div>

<script>
(function () {
  var buttons = document.querySelectorAll('.pub-filter-tabs button');
  var pubs = document.querySelector('.publications');

  function getYearBlocks() {
    return pubs ? pubs.querySelectorAll('h2.bibliography') : [];
  }

  function filterEntries(filter) {
    // Show/hide individual entries
    var entries = pubs ? pubs.querySelectorAll('.row[data-keywords]') : [];
    entries.forEach(function (el) {
      if (filter === 'all') {
        el.style.display = '';
      } else {
        var kw = (el.getAttribute('data-keywords') || '').split(',').map(function(s){ return s.trim(); });
        el.style.display = kw.indexOf(filter) !== -1 ? '' : 'none';
      }
    });

    // Hide year headings that have no visible entries
    var yearBlocks = getYearBlocks();
    yearBlocks.forEach(function (heading) {
      // Collect entries until the next heading
      var visible = false;
      var sib = heading.nextElementSibling;
      while (sib && sib.tagName !== 'H2') {
        if (sib.classList.contains('row') && sib.style.display !== 'none') {
          visible = true;
          break;
        }
        // Also check inside ol/ul wrappers the theme might generate
        var rows = sib.querySelectorAll('.row[data-keywords]');
        rows.forEach(function(r){ if (r.style.display !== 'none') visible = true; });
        sib = sib.nextElementSibling;
      }
      heading.style.display = visible ? '' : 'none';
    });
  }

  buttons.forEach(function (btn) {
    btn.addEventListener('click', function () {
      buttons.forEach(function (b) { b.classList.remove('active'); });
      btn.classList.add('active');
      filterEntries(btn.getAttribute('data-filter'));
    });
  });
})();
</script>
