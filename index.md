---
layout: default
title: null
permalink: /
---

<section id="latest-news" class="latest-news">
<h2>Latest News</h2>
<p>Follow us on <a href="https://bsky.app/profile/aiog.net" target="_blank" rel="me">Bluesky</a> for updates.</p>
<div class="bluesky-feed" id="bluesky-feed" data-bluesky-actor="aiog.net" data-bluesky-show="3">
<p class="bluesky-loading">Loading recent posts…</p>
</div>
</section>

<script>
(function() {
    var container = document.getElementById('bluesky-feed');
    if (!container) return;
    var actor = container.dataset.blueskyActor;
    var show = parseInt(container.dataset.blueskyShow, 10) || 3;
    var profileUrl = 'https://bsky.app/profile/' + actor;

    var url = 'https://public.api.bsky.app/xrpc/app.bsky.feed.getAuthorFeed'
        + '?actor=' + encodeURIComponent(actor)
        + '&limit=' + (show + 5)
        + '&filter=posts_no_replies';

    fetch(url)
        .then(function(r) { if (!r.ok) throw new Error('HTTP ' + r.status); return r.json(); })
        .then(function(data) {
            var seen = {};
            var posts = [];
            (data.feed || []).forEach(function(item) {
                var post = item.post;
                var reason = item.reason;
                if (reason && reason.$type === 'app.bsky.feed.defs#reasonRepost'
                    && reason.by && post.author && reason.by.did === post.author.did) return;
                if (seen[post.uri]) return;
                seen[post.uri] = 1;
                if (posts.length < show) posts.push(post);
            });
            container.innerHTML = '';
            if (posts.length === 0) {
                container.textContent = 'No recent posts.';
                return;
            }
            posts.forEach(function(p) { container.appendChild(renderPost(p)); });
        })
        .catch(function() {
            container.innerHTML = '';
            var p = document.createElement('p');
            p.className = 'bluesky-error';
            p.appendChild(document.createTextNode('Could not load posts. '));
            var a = document.createElement('a');
            a.href = profileUrl;
            a.target = '_blank';
            a.rel = 'noopener';
            a.textContent = 'View on Bluesky';
            p.appendChild(a);
            p.appendChild(document.createTextNode('.'));
            container.appendChild(p);
        });

    function renderPost(post) {
        var card = document.createElement('article');
        card.className = 'bluesky-post';
        var rkey = post.uri.split('/').pop();
        var postUrl = 'https://bsky.app/profile/' + post.author.handle + '/post/' + rkey;

        var text = document.createElement('div');
        text.className = 'bluesky-post__text';
        appendFacetedText(text, post.record.text || '', post.record.facets || []);
        card.appendChild(text);

        var embed = post.embed;
        if (embed && embed.$type === 'app.bsky.embed.images#view' && embed.images) {
            var row = document.createElement('div');
            row.className = 'bluesky-post__images';
            embed.images.slice(0, 4).forEach(function(img) {
                var link = document.createElement('a');
                link.href = postUrl;
                link.target = '_blank';
                link.rel = 'noopener';
                var i = document.createElement('img');
                i.src = img.thumb;
                i.alt = img.alt || '';
                i.loading = 'lazy';
                link.appendChild(i);
                row.appendChild(link);
            });
            card.appendChild(row);
        }

        var footer = document.createElement('div');
        footer.className = 'bluesky-post__footer';
        var fLink = document.createElement('a');
        fLink.href = postUrl;
        fLink.target = '_blank';
        fLink.rel = 'noopener';
        fLink.textContent = formatDate(post.record.createdAt);
        footer.appendChild(fLink);
        card.appendChild(footer);
        return card;
    }

    function appendFacetedText(parent, text, facets) {
        var enc = new TextEncoder();
        var dec = new TextDecoder();
        var bytes = enc.encode(text);
        var sorted = (facets || []).slice().sort(function(a, b) {
            return a.index.byteStart - b.index.byteStart;
        });
        var cursor = 0;
        sorted.forEach(function(f) {
            var bs = f.index.byteStart, be = f.index.byteEnd;
            if (bs > cursor) parent.appendChild(document.createTextNode(dec.decode(bytes.slice(cursor, bs))));
            var seg = dec.decode(bytes.slice(bs, be));
            var feats = f.features || [];
            var link = find(feats, 'app.bsky.richtext.facet#link');
            var tag = find(feats, 'app.bsky.richtext.facet#tag');
            var mention = find(feats, 'app.bsky.richtext.facet#mention');
            if (link) parent.appendChild(makeLink(seg, link.uri));
            else if (tag) parent.appendChild(makeLink(seg, 'https://bsky.app/hashtag/' + encodeURIComponent(tag.tag)));
            else if (mention) parent.appendChild(makeLink(seg, 'https://bsky.app/profile/' + mention.did));
            else parent.appendChild(document.createTextNode(seg));
            cursor = be;
        });
        if (cursor < bytes.length) parent.appendChild(document.createTextNode(dec.decode(bytes.slice(cursor))));
    }

    function find(arr, type) {
        for (var i = 0; i < arr.length; i++) if (arr[i].$type === type) return arr[i];
        return null;
    }

    function makeLink(text, href) {
        var a = document.createElement('a');
        a.href = href;
        a.target = '_blank';
        a.rel = 'noopener';
        a.textContent = text;
        return a;
    }

    function formatDate(iso) {
        var d = new Date(iso);
        if (isNaN(d)) return '';
        var now = new Date();
        var diff = now - d;
        var day = 86400000;
        if (diff < day) return 'today';
        if (diff < 2 * day) return 'yesterday';
        if (diff < 7 * day) return Math.floor(diff / day) + 'd ago';
        var opts = { month: 'short', day: 'numeric' };
        if (d.getFullYear() !== now.getFullYear()) opts.year = 'numeric';
        return d.toLocaleDateString('en-US', opts);
    }
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
