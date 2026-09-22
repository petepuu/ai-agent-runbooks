---
name: hr-email-reply
description: |-
  Takes the knowledge article(s) in ServiceNow HR Service Delivery and writes a short, 
  friendly email body for the employee. Produces just HTML version (with a real `<table>` for sources). 
  Use when asked to "turn this ServiceNow response into an email", "format this KB article as an HR reply", 
  "write the employee email from the ServiceNow result", or "give me an HTML version of this
  HR email". Do NOT use for general email drafting or for searching ServiceNow itself.
metadata:
  category: communication
  icon: MailEdit
---
## Purpose

Employee has asked HR question and  s the response in hand. This skill turns it into a
plain-language email (subject + body) the agent can send only as HTML; everything in
the email comes from the ServiceNow text. It is a sample: copy it into your HR agent and adjust tone.

## When NOT to Use

- General or non-HR email drafting — use the standard email drafting skill.
- IT, facilities or finance tickets — different service desk, different knowledge base.

## Sample ServiceNow response (one or more articles)

```json
{
  "articles": [
    {
      "kb_number": "KB0010234",
      "title": "Parental leave – duration and how to apply",
      "text": "Employees are entitled to 16 weeks of parental leave. Apply in the HR portal under Time Off > Parental leave at least 4 weeks before the start date. Your manager approves the request.",
      "url": "https://hr.service-now.com/kb?id=KB0010234"
    },
    {
      "kb_number": "KB0010587",
      "title": "Parental leave – pay during leave",
      "text": "Parental leave is paid in full for all 16 weeks, on the usual pay date.",
      "url": "https://hr.service-now.com/kb?id=KB0010587"
    }
  ]
}
```

## Steps

1. **Read the ServiceNow answer.** Note each article's KB number, title, text and link.
2. **Write the answer email** from the matching article(s) using the template below; if part of the question is not covered, say so in the email.
3. **Fill the template** with the employee's first name, users questions and the answer in plain words, and one next step taken from an article including citations (reference) numbers like [1]
5. **Pick the format:** output always as HTML
6. **Check before sending:** every fact is in the ServiceNow text, no JSON leaked, reads in under a minute.

## Output format: HTML template

```html
<p>Hi &lt;First name&gt;,</p>
<p>Question: '&lt;user's exact question&gt;'</p>
<p>&lt;Short answer, 2–4 sentences, based only on the article content &gt;</p>
<p>Next step: &lt;one action from an article&gt;.</p>
<p>Sources:</p>
<table border="1" cellpadding="4" cellspacing="0">
  <tr><th>KB number</th><th>Title</th><th>Link</th></tr>
  <tr><td>&lt;KB number&gt;</td><td>&lt;Title&gt;</td><td><a href="&lt;URL&gt;">&lt;URL&gt;</a></td></tr>
</table>
<p>Kind regards,<br>[HR team]</p>
```

## Worked example

Anna asks: "How long is parental leave, is it paid, and how do I apply?" — ServiceNow returns both sample articles.


Same email as HTML:
```html
<p>Hi Anna,</p>
<p>Question: 'How long is parental leave, is it paid, and how do I apply?'</p>
<p>You are entitled to 16 weeks of parental leave, paid in full for all 16 weeks on your
usual pay date[1]. To take it, submit a request in the HR portal under Time Off &gt; Parental
leave at least 4 weeks before your planned start date; your manager will then approve it.</p>
<p>Next step: open the HR portal and submit your Parental leave request[2].</p>
<p>Sources:</p>
<table border="1" cellpadding="4" cellspacing="0">
  <tr><th>Ref</th><th>KB number</th><th>Title</th><th>Link</th></tr>
  <tr><td>[1]</td><td>Parental leave – duration and how to apply</td><td><a href="https://hr.service-now.com/kb?id=KB0010234">https://hr.service-now.com/kb?id=KB0010234</a></td></tr>
  <tr><td>[2]</td><td>Parental leave – pay during leave</td><td><a href="https://hr.service-now.com/kb?id=KB0010587">https://hr.service-now.com/kb?id=KB0010587</a></td></tr>
</table>
<p>Kind regards,<br>[HR team]</p>
```

## Guardrails

- Never invent anything not in the ServiceNow text; if something is missing, say so in the email.
- Never paste internal field names, JSON or system IDs (other than the KB numbers) into the email.
- List each cited article once in the sources table including the reference number.
- In the HTML version, escape any `<`/`>`/`&` from the source text and always use a real `<a href>` link.
- Review before sending: one short answer, KB numbers inline, one next step, one sources table.
