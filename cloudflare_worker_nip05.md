# Pre-Requisit:
- Domain in Cloudflare verfügbar (https://developers.cloudflare.com/registrar/get-started/transfer-domain-to-cloudflare/)

# Worker erstellen
1. Globale Navigation > Workers&Pages > Create > Create Worker > Name eingeben > Deploy

2. Edit Code > Codeblock einfügen

    > NOSTR_NPUB_HEX_ENCODED = https://nostrcheck.me/converter/

    > NOSTR_USERNAME = display name (alternativ kann auch _ eingegeben werden, um gegen die Domain zu validieren)

    > Relays sind optional 

<code>

    addEventListener('fetch', event => {
        event.respondWith(handleRequest(event.request))
    })

    const nostrjson = `{
        "names": {
            "<<NOSTR_USERNAME>>": "<<NOSTR_NPUB_HEX_ENCODED>>"
        },
        "relays": {
            "<<NOSTR_NPUB_HEX_ENCODED>>": [
                "wss://<<NOSTR_RELAY_URL>>"
            ]
        }
    }`;

    async function handleRequest(request) {
        return new Response(nostrjson, {
            status: 200,
            headers: {
                'Access-Control-Allow-Origin': '*',
                'Content-Type': 'application/json',
            }
        });
    }

</code>

3. Deploy

# Workers Routes setzen
1. Domain öffnen (Global Navigation > Websites > Domain wählen)
2. Workers Routes > Add route > "example.com/.well-known/nostr.json*"
3. Vorher erstellten Worken auswählen

# Troubleshooting
1. In Nostr client überprüfen
2. https://example.com/.well-known/nostr.json?name=username => Output im Browser überprüfen
