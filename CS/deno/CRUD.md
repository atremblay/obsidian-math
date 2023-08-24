
## POST 

Here are two ways. It's not specific to `POST`, I just happened to toy around this while writing this method.

`body.value` is an object of type `Promise`. It promises to eventually deliver something. We can have the code block until we get that somthing with the `await` keyword with this line

```typescript
const activity = await body.value;
```

Then we can use that value directly (`json` in this case).

```typescript
router.post("/api/v1/activites", async ({
  request,
  response,
}: {
  request: any;
  response: any;
}) => {
  if (!request.hasBody) {
    response.status = 400;
    response.body = {
      success: false,
      msg: "No data provided",
    };
  } else {
    const body = request.body();
    let activityData: any;
    const activity = await body.value;

    activity.id = uuidv4();
    activities.push(activity);
    activityData = activity;

    response.status = 201;
    response.body = {
      success: true,
      data: activityData,
    };
  }
});
```

Or we can play with the `Promise` object by using the keyword `then`. When the `Promise` is resolved, it will then run this code. Again, we have to put an `await` in order to retrieve `activityData`. Otherwise the execution continues, so the response is returned without waiting for this.

```typescript
    const value = body.value;

    await value.then((activity: any) => {
      ...
    });
```

With `await`, we can add `activityData` in the response body. If it was not for this, `activityData` would be undefined and `data` would not even be part of the reponse body.

```typescript
router.post("/api/v1/activites", async ({
  request,
  response,
}: {
  request: any;
  response: any;
}) => {
  if (!request.hasBody) {
    response.status = 400;
    response.body = {
      success: false,
      msg: "No data provided",
    };
  } else {
    const body =  request.body();
    let activityData: any;
    const value = body.value;

    await value.then((activity: any) => {
      activity.id = uuidv4();
      activities.push(activity);
      activityData = activity;
    });

    response.status = 201;
    response.body = {
      success: true,
      data: activityData,
    };
  }
});
```