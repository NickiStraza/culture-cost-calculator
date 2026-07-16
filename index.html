const crypto = require('crypto');

const CORS = { 'Access-Control-Allow-Origin': '*' };
const LIST_ID = '4c288a4b41';
const LEAD_TAG = 'Culture Cost Calculator';

exports.handler = async function(event) {
  if (event.httpMethod !== 'POST') {
    return { statusCode: 405, body: 'Method not allowed' };
  }

  try {
    const body = JSON.parse(event.body);
    const email = (body.email || '').trim().toLowerCase();
    const firstName = body.firstName || '';

    if (!email) {
      return { statusCode: 400, headers: CORS, body: JSON.stringify({ error: 'Email required.' }) };
    }

    const apiKey = process.env.MAILCHIMP_API_KEY;
    const dc = apiKey.split('-').pop(); // e.g. "us21", taken from the end of the API key
    const auth = 'Basic ' + Buffer.from('anystring:' + apiKey).toString('base64');
    const subscriberHash = crypto.createHash('md5').update(email).digest('hex');

    // Add or update the contact (PUT is safe to call even if they already exist)
    const memberResponse = await fetch(
      `https://${dc}.api.mailchimp.com/3.0/lists/${LIST_ID}/members/${subscriberHash}`,
      {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json', 'Authorization': auth },
        body: JSON.stringify({
          email_address: email,
          status_if_new: 'subscribed',
          merge_fields: { FNAME: firstName }
        })
      }
    );
    const memberData = await memberResponse.json();

    if (!memberResponse.ok) {
      console.error('Mailchimp member error:', memberResponse.status, JSON.stringify(memberData));
      return {
        statusCode: memberResponse.status,
        headers: CORS,
        body: JSON.stringify({ error: 'Subscribe failed.' })
      };
    }

    // Apply the source tag (separate endpoint; tags aren't set reliably through the member PUT above)
    const tagResponse = await fetch(
      `https://${dc}.api.mailchimp.com/3.0/lists/${LIST_ID}/members/${subscriberHash}/tags`,
      {
        method: 'POST',
        headers: { 'Content-Type': 'application/json', 'Authorization': auth },
        body: JSON.stringify({ tags: [{ name: LEAD_TAG, status: 'active' }] })
      }
    );

    if (!tagResponse.ok) {
      const tagData = await tagResponse.json().catch(() => ({}));
      console.error('Mailchimp tag error:', tagResponse.status, JSON.stringify(tagData));
      // Contact was still added successfully, so we don't fail the whole request over a tag issue
    }

    return { statusCode: 200, headers: CORS, body: JSON.stringify({ success: true }) };
  } catch (err) {
    console.error('Function error:', err.message);
    return { statusCode: 500, headers: CORS, body: JSON.stringify({ error: 'Subscribe failed.' }) };
  }
};
