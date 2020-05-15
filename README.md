# How to install PostgreSQL 12 and PostGIS 3 on AWS Linux 2 EC2 instance
Trying to install PostGIS libraries/binaries from the pre-built RedHat/CentOS yum repository
is difficult on an AWS Linux 2 EC2 instance. Therefore, here are some instructions on
how to build and install all required software for PostGIS 3 libraries
on an AWS Linux 2 EC2 instance. 

Installing the latest PostGIS 3 libraries/binaries on an EC2 instance
is necessary when your AWS RDS Postgres 12 instance is located behind a private subnet. Your public EC2 Linux
instance needs the necessary PostgreSQL 12 server/client and PostGIS 3 libraries/binaries in order
to access the private AWS RDS instance.

These instructions could also be used to install Postgres 12 and PostGIS 3 geospatial database
on a standalone AWS Linux 2 EC2 instance.

At the time of this writing, PostgreSQL 12.2 and PostGIS 3.0.1 is the latest version.

## Log into AWS EC2 Linux 2 Instance

## Install YUM libraries needed for source compilation
```
sudo yum -y install gcc gcc-c++ make cmake libtool libcurl-devel libxml2-devel rubygems swig fcgi-devel\
                    libtiff-devel freetype-devel curl-devel libpng-devel giflib-devel libjpeg-devel \
                    cairo-devel freetype-devel readline-devel openssl-devel python27 python27-devel \
                    json-c json-c-devel
```

## PostgreSQL 12
```
wget http://ftp.postgresql.org/pub/source/v12.2/postgresql-12.2.tar.gz
tar xvf postgresql-12.2.tar.gz
cd postgresql-12.2/
./configure --with-openssl --bindir=/usr/bin
make
sudo make install
cd ..
```

## PROJ-4 reprojection library
Proj4 reprojection library, version 4.6.0 or greater. Proj4 4.9 or above is needed to take advantage of improved
geodetic. The Proj4 library is used to provide coordinate reprojection support within PostGIS. Proj4 is available 
for download from http://trac.osgeo.org/proj/
```
wget http://download.osgeo.org/proj/proj-4.9.3.tar.gz
tar xvf proj-4.9.3.tar.gz
cd proj-4.9.3
./configure
make
sudo make install
cd ..
```

## GEOS geometry library
GEOS geometry library, version 3.6 or greater, but GEOS 3.7+ is recommended to take full advantage of all the new
functions and features. GEOS is available for download from http://trac.osgeo.org/geos/
```
wget http://download.osgeo.org/geos/geos-3.8.1.tar.bz2
tar xvf geos-3.8.1.tar.bz2
cd geos-3.8.1
./configure
make
sudo make install
cd ..
```

## GDAL
GDAL version 1.8 or higher (1.9 or higher is strongly recommended since some things will not work well or behave
differently with lower versions). This is required for raster support. 
http://trac.osgeo.org/gdal/wiki/DownloadSource
```
wget http://download.osgeo.org/gdal/2.4.4/gdal-2.4.4.tar.gz
tar xvf gdal-2.4.4.tar.gz
cd gdal-2.4.4
./configure
make
sudo make install
cd ..
```

## PostGIS 3
```
export LD_LIBRARY_PATH=/usr/local/pgsql/lib
wget http://download.osgeo.org/postgis/source/postgis-3.0.1.tar.gz
cd postgis-3.0.1
./configure
make
sudo make install
cd ..
```
