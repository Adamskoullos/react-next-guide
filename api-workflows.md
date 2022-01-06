# API Workflows

1. Create API file within `services` folder for the specific base url and then create the api for each endpoint:

```js
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

const cryptoNewsHeaders = {
  "x-bingapis-sdk": "true",
  "x-rapidapi-host": process.env.REACT_APP_NEWS_RAPIDAPI_HOST,
  "x-rapidapi-key": process.env.REACT_APP_RAPIDAPI_KEY,
};

const baseUrl = process.env.REACT_APP_NEWS_API_URL;

const createRequest = (url) => ({ url, headers: cryptoNewsHeaders });

export const cryptoNewsApi = createApi({
  reducerPath: "cryptoNewsApi",
  baseQuery: fetchBaseQuery({ baseUrl }),
  endpoints: (builder) => ({
    getCryptoNews: builder.query({
      query: ({ newsCategory, count }) =>
        createRequest(
          `/news/search?q=${newsCategory}&safeSearch=Off&textFormat=Raw&freshness=Day&count=${count}`
        ),
    }),
  }),
});

export const { useGetCryptoNewsQuery } = cryptoNewsApi;
```

2. Use the API within component, in this case the parent component:

```js
import { Grid, Typography } from "@mui/material";
import NewsCard from "../components/NewsCard";
import { useGetCryptoNewsQuery } from "../services/cryptoNewsApi";

const News = ({ simplified }) => {
  const { data: cryptoNews } = useGetCryptoNewsQuery({
    newsCategory: "Cryptocurrency",
    count: simplified ? 10 : 50,
  });

  console.log(cryptoNews);

  if (!cryptoNews?.value) return "Loading...";
  return (
    <div style={{ height: "100%" }}>
      <Typography variant="h1" color="primary" sx={{ pt: "5vh" }}>
        News Page
      </Typography>
      <Grid container spacing={4} sx={{ mt: 3 }}>
        {cryptoNews.value.map((item, index) => (
          <NewsCard item={item} key={index} i={index} />
        ))}
      </Grid>
    </div>
  );
};

export default News;
```
