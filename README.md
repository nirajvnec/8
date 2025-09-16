API calls to `/api/eventdata/SomeEvent` returned generic "404 Undocumented" errors
- The same endpoints worked perfectly in local development
- Istio VirtualService was configured to only accept requests starting with `/referencedata` prefix