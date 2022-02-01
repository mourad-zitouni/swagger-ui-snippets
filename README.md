# swagger-ui-snippets
Swagger ui doc with code snippets
Implements example swagger snippets :
https://codesandbox.io/s/yjjgj?file=/public/index.html



getSnippetGenerators: (ori, system) => (state, ...args) => {
          let test = ori(state, ...args);
          for (let i = 0; i < 3; i++) {
            test = test
              // add node native snippet generator
              .set(
                // key
                `node_native${i}`,
                // config and generator function
                system.Im.fromJS({
                  title: "NodeJs Native",
                  syntax: "javascript",
                  fn: (req) => {
                    // get extended info about request
                    const { spec, oasPathMethod } = req.toJS();
                    const { path, method } = oasPathMethod;

                    // run OpenAPISnippet for target node
                    const targets = ["node_native"];
                    let snippet;
                    try {
                      // set request snippet content
                      snippet = OpenAPISnippet.getEndpointSnippets(
                        spec,
                        path,
                        method,
                        targets
                      ).snippets[0].content;
                    } catch (err) {
                      // set to error in case it happens the npm package has some flaws
                      snippet = JSON.stringify(snippet);
                    }
                    // return stringified snipped
                    return snippet;
                  }
                })
              );
          }
          return test;
        }
