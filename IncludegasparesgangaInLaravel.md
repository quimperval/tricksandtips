# Include gasparesganga/php-shapefile in a laravel project

I added the library in the laravel project with composer require gasparesganga/php-shapefile
Once the command has completed you should have the gasparesganga in the vendor folder. 

Now you're able to use it where you want. 

I used it in a controller, first you need to import the classed you'll use: 


    use Shapefile\Shapefile;
    use Shapefile\ShapefileException;
    use Shapefile\ShapefileWriter;
    use Shapefile\Geometry\Point;

Then in your controller you can use it in any function, for example the below function: 

I have a *shapes* folder configured to store the shapefiles.
 
    public testFunction(){

        $PointA = new Point(226339.07747614052, 2332310);
        
        $zipFolder = \Storage::disk('shapes')->path('');
        //File name should have maximum 10 characters
        $fileName = "example";
        
        $shapeFilePath = $zipFolder.$fileName.".shp";
        
        try{
        
        $Shapefile = new ShapefileWriter($shapeFilePath); 
        $Shapefile->setShapeType(Shapefile::SHAPE_TYPE_POINT);
        
        $Shapefile->addNumericField('ID', 10);
        
        $Shapefile->addCharField('DESC', 25);
        
        $PointA->setData('ID', 1);
        
        $PointA->setData('DESC', "Point number 1");
        
        // Write the record to the Shapefile
        $Shapefile->writeRecord($PointA);
        
        unset($Shapefile);
        
        }catch (ShapefileException $e) {
           
            $data = [
                'code'=>500,
                'status'=>'error with ShapefileException',
                'message'=>'File not found'
            
            ];
        
            return response()->json($data, $data['code']);
        }catch (Exception $e){
            throw new Exception( 'Unable to upload file',0,$e);
        } 
    } 
