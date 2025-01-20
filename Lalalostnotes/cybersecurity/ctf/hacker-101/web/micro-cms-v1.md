---
description: One of the challenge of Hacker One Exploring XSS attack.
---

# Micro-CMS v1

## First Flag

I tried to explore about the feature of the web and the hint gives me an idea

<figure><img src="../../../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

We would like to check for the a new page and see how its respond

<figure><img src="../../../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

We load the payloads and send them into Caido

{% hint style="info" %}
You can use Burpsuite, ZAP, etc
{% endhint %}

<figure><img src="../../../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

We see that the payload is seems to stored on the server. Now we check it for the reponses:

<figure><img src="../../../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

Assuming the server stores payloads, they are displayed on the web repeatedly everytime when we visit the paged. Now we know that, it's XSS stored vulnerability!

Tried this payload for the new page:

<figure><img src="../../../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

```javascript
<script>alert('Give me your sigma)</script>
```

Then we go to home and acces the page again

<figure><img src="../../../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

We got first flag!

<details>

<summary>Flag</summary>

^FLAG^61f6ec1265ed9b2a9b85f19b67db5f8479d956d73afccbc42a3b604c1e7dcd8f$FLAG$

</details>



## Second Flag

Notice the page ID change. Initially, there are only 2 pages with IDs 1 and 2. Upon adding a new page, its ID starts at 6, indicating missing pages. We need to identify these lost pages.

<figure><img src="../../../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

We got the flag on the page 4, but you should add the edit directory first:

<figure><img src="../../../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Flag</summary>

^FLAG^1a525c40361d4064b7711007a53dd68f838b66c36ca24b5672109c46b3645082$FLAG$

</details>







I encourage you to do it yourself to understand how it works.

Happy Hacking!
