// @ts-nocheck
import { async } from '@angular/core/testing';
import { Observable, of as observableOf, throwError } from 'rxjs';

import { SdhCustomReuseStrategy } from './sdh-custom-reuse-strategy';
describe('SdhCustomReuseStrategy', () => {
  let obj;

  beforeEach(() => {
    obj = new SdhCustomReuseStrategy();
  });

  it('should run #shouldDetach()', async () => {

    obj.shouldDetach({
      routeConfig: {
        loadChildren: {}
      },
      data: {
        reuseRoute: {}
      }
    });

  });

  it('should run #getRouteUrl()', async () => {

    obj.getRouteUrl({
      _routerState: {
        url: {}
      }
    });

  });

  it('should run #store()', async () => {
    obj.storedRouteHandles = obj.storedRouteHandles || {};
    obj.storedRouteHandles.set = jest.fn();
    obj.getRouteUrl = jest.fn();
    obj.store({}, {});
    // expect(obj.storedRouteHandles.set).toHaveBeenCalled();
    // expect(obj.getRouteUrl).toHaveBeenCalled();
  });

  it('should run #shouldAttach()', async () => {
    obj.storedRouteHandles = obj.storedRouteHandles || {};
    obj.storedRouteHandles.has = jest.fn();
    obj.getRouteUrl = jest.fn();
    obj.shouldAttach({});
    // expect(obj.storedRouteHandles.has).toHaveBeenCalled();
    // expect(obj.getRouteUrl).toHaveBeenCalled();
  });

  it('should run #retrieve()', async () => {
    obj.storedRouteHandles = obj.storedRouteHandles || {};
    obj.storedRouteHandles.get = jest.fn();
    obj.getRouteUrl = jest.fn();
    obj.retrieve({
      routeConfig: {
        loadChildren: {}
      }
    });
    // expect(obj.storedRouteHandles.get).toHaveBeenCalled();
    // expect(obj.getRouteUrl).toHaveBeenCalled();
  });

  it('should run #shouldReuseRoute()', async () => {

    obj.shouldReuseRoute({
      routeConfig: {}
    }, {
      routeConfig: {}
    });

  });

  it('should run #clearCacheEntries()', async () => {
    obj.storedRouteHandles = obj.storedRouteHandles || {};
    obj.storedRouteHandles = ['storedRouteHandles'];
    obj.storedRouteHandles.delete = jest.fn();
    obj.clearCacheEntries({});
    // expect(obj.storedRouteHandles.delete).toHaveBeenCalled();
  });

});
