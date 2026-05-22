---
layout: default
title: null
permalink: /
description: The 1st AI & Open Government Workshop (AIOG) at ICAIL 2026, Singapore — exploring how AI and LLMs can advance open government, FOIA, and public disclosure laws worldwide.
---

<aside class="papers-spotlight" aria-labelledby="spotlight-heading">
    <div class="spotlight-text">
        <p class="spotlight-eyebrow">New · May 2026</p>
        <h2 id="spotlight-heading">Camera-ready PDFs of all 14 accepted papers are now available.</h2>
    </div>
    <div class="spotlight-actions">
        <a class="spotlight-cta" href="/accepted-papers/">Browse papers →</a>
        <a class="spotlight-cta-secondary" href="https://openreview.net/group?id=smu.edu.sg/SMU/ICAIL/2026/Workshop/AIOG#tab-accept" target="_blank" rel="noopener">OpenReview ↗</a>
    </div>
</aside>

<section id="latest-news" class="latest-news">
<h2>Latest News</h2>
<p>Follow us on <a href="https://bsky.app/profile/aiog.net" target="_blank" rel="me">Bluesky</a> for updates.</p>
<div class="bluesky-feed" id="bluesky-feed">
<p class="bluesky-loading">Loading latest post…</p>
</div>
</section>

<script>
(function() {
    var container = document.getElementById('bluesky-feed');
    if (!container) return;
    var actor = 'aiog.net';
    var url = 'https://public.api.bsky.app/xrpc/app.bsky.feed.getAuthorFeed'
        + '?actor=' + encodeURIComponent(actor)
        + '&limit=10&filter=posts_no_replies';

    fetch(url)
        .then(function(r) { if (!r.ok) throw new Error('HTTP ' + r.status); return r.json(); })
        .then(function(data) {
            var post = null;
            (data.feed || []).some(function(item) {
                if (item.reason) return false;
                post = item.post;
                return true;
            });
            if (!post) {
                container.innerHTML = '';
                return;
            }
            var rkey = post.uri.split('/').pop();
            var did = post.author.did;
            var handle = post.author.handle;
            var displayName = post.author.displayName || handle;
            var profileUrl = 'https://bsky.app/profile/' + did + '?ref_src=embed';
            var postUrl = 'https://bsky.app/profile/' + did + '/post/' + rkey + '?ref_src=embed';
            var date = new Date(post.record.createdAt);
            var dateText = isNaN(date) ? '' : date.toLocaleDateString('en-US', {
                day: 'numeric', month: 'long', year: 'numeric'
            });

            var bq = document.createElement('blockquote');
            bq.className = 'bluesky-embed';
            bq.setAttribute('data-bluesky-uri', post.uri);
            bq.setAttribute('data-bluesky-cid', post.cid);
            bq.setAttribute('data-bluesky-embed-color-mode', 'system');

            var p = document.createElement('p');
            var langs = post.record.langs;
            p.setAttribute('lang', (langs && langs[0]) || 'en');
            p.textContent = post.record.text || '';
            bq.appendChild(p);

            bq.appendChild(document.createTextNode(' — ' + displayName + ' ('));
            var profA = document.createElement('a');
            profA.href = profileUrl;
            profA.textContent = '@' + handle;
            bq.appendChild(profA);
            bq.appendChild(document.createTextNode(') '));
            var dateA = document.createElement('a');
            dateA.href = postUrl;
            dateA.textContent = dateText;
            bq.appendChild(dateA);

            container.innerHTML = '';
            container.appendChild(bq);

            var s = document.createElement('script');
            s.src = 'https://embed.bsky.app/static/embed.js';
            s.async = true;
            s.charset = 'utf-8';
            document.body.appendChild(s);
        })
        .catch(function() {
            container.innerHTML = '';
            var p = document.createElement('p');
            p.className = 'bluesky-error';
            p.appendChild(document.createTextNode('Could not load latest post. '));
            var a = document.createElement('a');
            a.href = 'https://bsky.app/profile/aiog.net';
            a.target = '_blank';
            a.rel = 'noopener';
            a.textContent = 'View on Bluesky';
            p.appendChild(a);
            p.appendChild(document.createTextNode('.'));
            container.appendChild(p);
        });
})();
</script>

<div class="dates-widget" markdown="1">

### Important Dates

| | |
|---|---|
| Paper submission deadline | **~~April 16, 2026~~** |
| Notification of acceptance | **~~May 4, 2026~~** |
| Camera-ready deadline | **May 20, 2026** |
| Workshop date | **June 8, 2026** |

[View all dates](/dates/)

</div>

<section id="objectives" markdown="1">

## Objectives

In many jurisdictions, open government and access to information laws such as Freedom of Information (FOIA) and Access to Information (ATI) require large-scale public disclosure of government records, resulting in massive, multimodal data collections whose complexity increasingly challenges both legal compliance and technical processing ([van Heusden et al., 2025](https://doi.org/10.1038/s41597-025-05052-2)). At the same time, governments face strict legal obligations to disclose information within statutory deadlines, while protecting sensitive and personal information. This raises AI & Law questions about how to operationalise legal standards for disclosure, sensitivity, and privacy in AI systems, and how to ensure auditability, explainability, and accountability of AI-assisted disclosure workflows.

The AI & Open Government workshop will focus on how modern AI tools and techniques, including Large Language Models (LLMs), can support government accountability and transparency by improving public access to government records and enabling more reliable and compliant disclosure processes ([Trippas et al., 2025](https://doi.org/10.1145/3769733.3769739)). The workshop addresses both perspectives of open government:

1. AI tools and techniques for improving search, exploration, and understanding of public government information, and
2. AI for assisting governments in improved accessibility, pre-processing, metadata enrichment, retrieval, filtering, and protecting sensitive information consistent with public disclosure laws prior to release.

The workshop will solicit written submissions, including original research papers and position papers offering insights from practice, and will facilitate structured dialogue to advance shared understanding of the uses, risks, and opportunities of AI in open government contexts. Papers will be peer-reviewed by the workshop's program committee and published in OpenReview proceedings.

</section>

<section id="workshop-structure" markdown="1">

## Workshop Structure

The workshop will be held as a full-day pre-conference event, structured in four parts:

<div class="workshop-parts">
    <div class="workshop-part">
        <h3>Part I: Introduction</h3>
        <p>An introductory overview of AI in large public data collections, including recent legal and policy developments, featuring two keynote talks (one from a legal or archival practitioner, one from a data scientist) and a short agenda-setting panel.</p>
    </div>
    <div class="workshop-part">
        <h3>Part II: Paper Presentations</h3>
        <p>Oral presentations of accepted research and position papers illustrating a range of AI and LLM approaches, followed by a facilitated discussion to identify key open research and standards issues.</p>
    </div>
    <div class="workshop-part">
        <h3>Part III: Breakout Sessions</h3>
        <p>Breakout sessions led by the organizers and invited guests, in which participants will work towards articulating best practices and open challenges for AI-assisted disclosure and sensitivity review.</p>
    </div>
    <div class="workshop-part">
        <h3>Part IV: Synthesis & Next Steps</h3>
        <p>A concluding panel and plenary discussion synthesizing breakout outcomes and outlining a research agenda and community-building steps, building from two central questions: What research questions should be explored to further the development of novel techniques and best practices for AI in furtherance of open government and public access to governmental records? And who, beyond those already in the room, do we need to engage to address the issues we have identified?</p>
    </div>
</div>

</section>

<section id="previous" markdown="1">

## Previous Editions

This workshop continues a well-established ICAIL tradition of AI for large-scale legal document analysis, initiated by the Discovery of Electronically Stored Information (DESI) workshops (2007–2017) and later extended by the Legal AI and Intelligent Assistance (Legal AIIA) workshops (2019–2023). These workshops brought together lawyers, legal professionals, information science scholars, and industry representatives to study AI methods for identifying responsive records in litigation and regulatory contexts. AI & Open Government builds on this background but shifts the focus to open government and public disclosure regimes, where the legal standards, stakeholders, and risks differ from e-discovery, and where public access, transparency, and democratic accountability are central concerns.

</section>
