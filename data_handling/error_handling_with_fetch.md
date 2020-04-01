# Error Handling With Fetch API

```js
function makeRequest(url, options) {
    return new Promise((resolve, reject) => {
        fetch(url, options)
            .then(handleResponse)
            .then(response => JSON.parse(response))
            .then((json) => resolve(json))
            .catch((error) => {
                try {
                    reject(JSON.parse(error))
                }
                    catch(e) {
                        reject(error)
                    }
            })
    })
}

function handleResponse(response) {
    return response.json()
        .then((json) => {
            // Modify response to include status[ok/success/status/text]
            let modifiedJson = {
                success: response.ok,
                status: response.status,
                statusText: response.statusText ? response.statusText : json.error || '',
                response: json
            }
            
            // If request failed, reject and return modified json string as error
            if (! modifiedJson.success) return Promise.reject(JSON.stringify(modifiedJson))

            // If successful, continue by returning modified json string
            return JSON.stringify(modifiedJson)
        })
}
```

## Usage

```js
const options = {
  // method, cors, headers, etc...
}
makeRequest('/api/url', options)
  .then((data) => console.log(data))
  .catch(error => console.log(error))
```
