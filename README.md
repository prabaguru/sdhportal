typescript
import { ActivatedRouteSnapshot, DetachedRouteHandle } from '@angular/router';
import { SdhCustomReuseStrategy } from './path-to-your-file';

describe('SdhCustomReuseStrategy', () => {
    let reuseStrategy: SdhCustomReuseStrategy;

    beforeEach(() => {
        reuseStrategy = new SdhCustomReuseStrategy();
    });

    describe('shouldDetach', () => {
        it('should return true if route is marked for reuse', () => {
            const route: ActivatedRouteSnapshot = {
                routeConfig: { loadChildren: null },
                data: { reuseRoute: true }
            } as ActivatedRouteSnapshot;
            expect(reuseStrategy.shouldDetach(route)).toBe(true);
        });

        it('should return false if route is not marked for reuse', () => {
            const route: ActivatedRouteSnapshot = {
                routeConfig: { loadChildren: null },
                data: { reuseRoute: false }
            } as ActivatedRouteSnapshot;
            expect(reuseStrategy.shouldDetach(route)).toBe(false);
        });

        it('should return false if routeConfig is null', () => {
            const route: ActivatedRouteSnapshot = {
                routeConfig: null,
                data: { reuseRoute: true }
            } as ActivatedRouteSnapshot;
            expect(reuseStrategy.shouldDetach(route)).toBe(false);
        });
    });

    describe('store', () => {
        it('should store route handle correctly', () => {
            const route: ActivatedRouteSnapshot = {
                routeConfig: { loadChildren: null },
                data: { reuseRoute: true },
                params: {},
                queryParams: {},
                url: [],
                // other necessary properties
            } as ActivatedRouteSnapshot;

            const handle: DetachedRouteHandle = {};
            reuseStrategy.store(route, handle);
            expect(reuseStrategy.storedRouteHandles.size).toBe(1);
        });
    });

    describe('shouldAttach', () => {
        it('should return true if route is stored', () => {
            const route: ActivatedRouteSnapshot = {
                routeConfig: { loadChildren: null },
                data: { reuseRoute: true }
            } as ActivatedRouteSnapshot;

            const handle: DetachedRouteHandle = {};
            reuseStrategy.store(route, handle);
            expect(reuseStrategy.shouldAttach(route)).toBe(true);
        });

        it('should return false if route is not stored', () => {
            const route: ActivatedRouteSnapshot = {
                routeConfig: { loadChildren: null },
                data: { reuseRoute: true }
            } as ActivatedRouteSnapshot;

            expect(reuseStrategy.shouldAttach(route)).toBe(false);
        });
    });

    describe('retrieve', () => {
        it('should return stored handle if route is stored', () => {
            const route: ActivatedRouteSnapshot = {
                routeConfig: { loadChildren: null },
                data: { reuseRoute: true }
            } as ActivatedRouteSnapshot;

            const handle: DetachedRouteHandle = {};
            reuseStrategy.store(route, handle);
            expect(reuseStrategy.retrieve(route)).toBe(handle);
        });

        it('should return null if route is not stored', () => {
            const route: ActivatedRouteSnapshot = {
                routeConfig: { loadChildren: null },
                data: { reuseRoute: true }
            } as ActivatedRouteSnapshot;

            expect(reuseStrategy.retrieve(route)).toBeNull();
        });
    });

    describe('shouldReuseRoute', () => {
        it('should return true if future and current route configs are the same', () => {
            const future: ActivatedRouteSnapshot = {
                routeConfig: { path: 'test' },
            } as ActivatedRouteSnapshot;

            const current: ActivatedRouteSnapshot = {
                routeConfig: { path: 'test' },
            } as ActivatedRouteSnapshot;

            expect(reuseStrategy.shouldReuseRoute(future, current)).toBe(true);
        });

        it('should return false if future and current route configs are different', () => {
            const future: ActivatedRouteSnapshot = {
                routeConfig: { path: 'test' },
            } as ActivatedRouteSnapshot;

            const current: ActivatedRouteSnapshot = {
                routeConfig: { path: 'different' },
            } as ActivatedRouteSnapshot;

            expect(reuseStrategy.shouldReuseRoute(future, current)).toBe(false);
        });
    });

    describe('clearCacheEntries', () => {
        it('should clear cache entries matching the path', () => {
            const route1: ActivatedRouteSnapshot = {
                routeConfig: { loadChildren: null },
                data: { reuseRoute: true }
            } as ActivatedRouteSnapshot;
            const route2: ActivatedRouteSnapshot = {
                routeConfig: { loadChildren: null },
                data: { reuseRoute: true }
            } as ActivatedRouteSnapshot;

            reuseStrategy.store(route1, {});
            reuseStrategy.store(route2, {});
            reuseStrategy.clearCacheEntries('test');

            expect(reuseStrategy.storedRouteHandles.size).toBe(0); // Assuming both are stored under the same path
        });

        it('should not clear cache if no matching path', () => {
            const route: ActivatedRouteSnapshot = {
                routeConfig: { loadChildren: null },
                data: { reuseRoute: true }
            } as ActivatedRouteSnapshot;

            reuseStrategy.store(route, {});
            reuseStrategy.clearCacheEntries('nonexistent');

            expect(reuseStrategy.storedRouteHandles.size).toBe(1); // Should remain unchanged
        });
    });
});
