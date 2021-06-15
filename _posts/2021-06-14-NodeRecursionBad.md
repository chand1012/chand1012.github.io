---
layout: post
title: Why you should avoid long running recursion in Node.
---

I don't like recursion. I know its a controversial opinion, but I don't like it. I've had too many issues with recursive functions, plus my brain never really got the concept when I first started programming. I avoid using recursion whenever I can, only using in the most obvious of cases (like the classic factorial example).

Not long ago, I was working on a project for work when I noticed that there were tons of errors in the logs, as the lambda that was running the code kept on running out of memory. The code was in production, and as a temporary fix the RAM for the lambda was cranked from 1GB to 3GB, which would also aid in finding where the bug was coming from. This script was written in NodeJS 14, made to run on a lambda, and acted as a download script. The data being downloaded was gotten from an API that could only return chunks of data, but we needed the whole dataset to run our algorithms on. Our solution was to get the data as a JSON array, then save it to AWS S3, using it as a sort of database for the JSON files. I noticed that to download 100MB of data, the RAM use was well over 1.5GB. While you're almost never going to get a 1:1 data size to memory use ratio, it **should not** be as extreme as that. 

![High Memory Use](https://raw.githubusercontent.com/chand1012/chand1012.github.io/master/images/lambdaMemoryUseNode.jpg)

The shown example is quite extreme, as most of the time the data we're downloading doesn't go above 20MB, but there are edge cases were we could be downloading as much as 200MB. If the latter is the case, there's no way going to run as intended.

I did some searching, and I found [this](https://stackoverflow.com/a/16928870/5178731) StackOverflow post. It seems that Node's garbage collector doesn't clean up until after recursion is complete, and the recursion in this script did not end until *after the main purpose of the script had finished*. Here is the original recursive function code:

```javascript

const allMessages = [];

const objectId = "someObjectId";

const callAPI = async (cursor = null) => {
    const headers = {'X-Api-Key': 'someApiKey'};
    const url = `https://api.url.here/${objectId}/${
        cursor ? `?cursor=${cursor}` : ''
    }`;
    const resp = await fetch(url, { headers });
    const { _next, comments } = await resp.json();
    allMessages.push(...comments);

    if (_next) {
        await callAPI(_next);
    }
};

await callAPI();

```

The basic idea is that this API returned us a cursor to to paginate the JSON data we were retrieving and storing for later in S3. If the cursor returned null from the API, we knew this was the last page of data and we could break recursion. The solution to this issue was really simple.

```javascript
const allMessages = [];
const objectId = "someObjectId";

const callAPI = async (cursor = null) => {
    const headers = {'X-Api-Key': 'someApiKey'};
    const url = `https://api.url.here/${objectId}/${
        cursor ? `?cursor=${cursor}` : ''
    }`;
    const resp = await fetch(url, { headers });
    const { _next, comments } = await resp.json();
    allMessages.push(...comments);

    return _next;
};

var cursor = await callAPI();

while (cursor) {
    cursor = await callAPI(cursor);
}
```

This achieves the exact same functionality while fixing the garbage collector problem of before. Rather than recursively executing, the function is called once before starting a `while` loop, which conditionally runs provided that `cursor` is not `null`, appending the data like before into `allMessages`.

This is not the main reason I avoided recursive functions, but it has definitely been added to the list. I (as well as the man who wrote this code) are definitely more wary about using recursive functions on lots of data or long running processes, as you should be as well.