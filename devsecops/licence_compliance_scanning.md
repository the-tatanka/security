# Licence compliance scanning

The <b>CycloneDX</b> module for Node.js is used to generate a Software Bill-of-Materials (BOM) containing an aggregate of all project dependencies.

- Github Action: https://github.com/CycloneDX/gh-node-module-generatebom

- Dev dependency: https://www.npmjs.com/package/@cyclonedx/bom

Only production dependencies are part of the BOM. The result is a JSON file called "bom.json".
