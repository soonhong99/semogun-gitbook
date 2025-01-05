# 클라이언트 SDK

## React Client SDK

```typescript
// @realestate/client-sdk
import { createPropertyClient } from '@realestate/client-sdk';

const client = createPropertyClient({
  baseURL: 'https://api.realestate.com',
  version: 'v1',
  credentials: {
    apiKey: 'your-api-key'
  }
});

// Property API Wrapper
export class PropertyAPI {
  async searchProperties(params: SearchParams): Promise<SearchResult> {
    return client.get('/properties/search', { params });
  }

  async uploadImages(propertyId: string, files: File[]): Promise<UploadResult> {
    const formData = new FormData();
    files.forEach(file => formData.append('images', file));
    return client.post(`/properties/${propertyId}/images`, formData);
  }
}
```

***

## Redux 통합

```typescript
// Redux Toolkit Query 설정
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

export const propertyApi = createApi({
  reducerPath: 'propertyApi',
  baseQuery: fetchBaseQuery({ 
    baseUrl: 'https://api.realestate.com/v1/',
    prepareHeaders: (headers, { getState }) => {
      const token = (getState() as RootState).auth.token;
      if (token) {
        headers.set('authorization', `Bearer ${token}`);
      }
      return headers;
    },
  }),
  endpoints: (builder) => ({
    getHotProperties: builder.query<Property[], void>({
      query: () => 'properties/hot',
      transformResponse: (response: ApiResponse<Property[]>) => response.data,
    }),
  }),
});
```
