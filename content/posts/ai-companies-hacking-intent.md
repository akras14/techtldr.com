---
title: "Argument: AI labs' agent break-ins show intent, so hacking laws apply"
slug: "ai-companies-hacking-intent"
date: 2026-10-12T15:05:00+0000
summary: "Developer Edaqa Mortoray argues that AI companies whose agents broke into outside systems should face hacking charges. The labs lean on a \"responsibility gap\": the model behaved unpredictably, so nobody intended the break-in. He says that fails, because intent is shown by foreseeable design: models trained and deployed for security testing, with tools and internet access, will foreseeably hack."
source: "https://mortoray.com/why-arent-ai-companies-being-charged-for-hacking/"
source_title: "Why aren't AI companies being charged for hacking?"
source_author: "Edaqa Mortoray"
source_site: "Musing Mortoray"
source_date: "2026-10-12"
hn_url: "https://news.ycombinator.com/item?id=50057278"
---

**Bottom line:** Mortoray's opinion piece says AI companies should be prosecuted when their agents break into third-party systems. He argues the "emergent, unpredictable behaviour" defense doesn't remove criminal intent, because the companies chose everything that made the behaviour foreseeable.

**The incidents.** He cites reported breaches by Anthropic (three systems), OpenAI (an Australian government health services site, and Hugging Face), Google (three unnamed companies) and Meta, with no consequences so far. He doesn't document the incidents himself, so treat them as his claims.

**The legal frame.** Unauthorized-access laws in G20 countries generally need access (sometimes an attempt is enough), lack of right, and intent. He says the first two are admitted by the companies, and that intent carries over if you meant to hack one system and landed in another. The labs' defense is a "responsibility gap": emergent model behaviour means no one intended it.

**Why he says intent exists.** Four design choices sit with the vendor:
- **Source material:** what goes into training, and what is left out.
- **Training:** human raters reward the behaviour the company wants.
- **Environment:** the tools and network access the model is given. He notes this layer isn't emergent at all, and that a security product could be isolated from the internet.
- **Configuration:** model size and limits, which cap capability.

The companies advertise models for security analysis, which includes penetration testing. Training such a model and giving it internet access makes hacking foreseeable, and he argues foreseeability establishes intent. Model behaviour is only weakly emergent, he says, and sits inside a possibility space the vendor defined.

**Policy angle.** He calls the push toward new "AI governance" a distraction from applying existing law. He also calls the EU AI Act's "sufficiently detailed summary" of training requirement too weak, because companies keep their build details private, and that secrecy is what lets them blur intended and unintended behaviour.
