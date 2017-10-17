```
/**
 更新图片
 */
- (void)updateImagePickerImgs {
    
    if (self.serviceList.picUrls.length <=0) return;
      SDImageCache *sdcache = [SDWebImageManager sharedManager].imageCache;
    NSArray *picurls = [self.serviceList.picUrls componentsSeparatedByString:@","];
    
    // 如果是1张直接取如果是多张可能在磁盘或内容中没有被sd加载呢
    if (picurls.count == 1) {
        
        [_pickerView refreshImagePickerViewWithPhotoArray:@[[sdcache imageFromDiskCacheForKey:picurls[0]]]];
        return;
    }
    
    
    // 如果多张使用SDWebImagePrefetcher 进行批量下载
    
    NSMutableArray *arrURLS = [NSMutableArray array];
    for (NSString *url in picurls) {
        [arrURLS addObject:[NSURL URLWithString:url]];
    }
    
    SDWebImagePrefetcher *pref =  [SDWebImagePrefetcher sharedImagePrefetcher];
  
    [pref prefetchURLs:arrURLS.copy progress:^(NSUInteger noOfFinishedUrls, NSUInteger noOfTotalUrls) {
        
    } completed:^(NSUInteger noOfFinishedUrls, NSUInteger noOfSkippedUrls) {
        NSMutableArray *tempImgs = [NSMutableArray array];
        for (NSString *picurl in picurls) {
            UIImage *img = [sdcache imageFromDiskCacheForKey:picurl];
            if (img) {
                [tempImgs addObject:img];
            }
        }
        // 更新
        [_pickerView refreshImagePickerViewWithPhotoArray:tempImgs.copy];
    }];
    

}
```