## Error Handling

https://softwareengineering.stackexchange.com/questions/209693/best-practices-to-create-error-codes-pattern-for-an-enterprise-project

```ts
type Error = {
  // general high level error
  // should be OK for outside user to see
  description: string;
  // identifying info that is more specific about what happened
  // can contain internal details and should be omitted in production responses
  detail: string;
  // a category code that can be safely shown in production
  // has meaning to internal dev team but not meaning externally
  errorCode: string;
  // a unique ID for this error
  // used to identify the exact instance in logs
  errorId: string;
  // used when you are wrapping an existing exception
  originalError: unknown;
};
```
