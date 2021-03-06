# This is the root ITK CMakeList file:
cmake_minimum_required(VERSION 3.5)

# This project is designed to be built outside the Insight source tree.
project(PolyDataCompression)

# Find SlicerExecutionModel
find_package(SlicerExecutionModel REQUIRED)
include(${SlicerExecutionModel_USE_FILE})

# Find ITK
set(ITK_IO_MODULES_USED
  ITKIOImageBase
  ITKIONRRD
  ITKIOCSV
  ITKIOGIPL
  ITKIOHDF5
  ITKIOIPL
  ITKIOImageBase
  ITKIOLSM
  ITKIOMRC
  ITKIOMesh
  ITKIOMeta
  ITKIONIFTI
  ITKIONRRD
  ITKIORAW
  ITKIOVTK
  )
find_package(ITK 4.7 REQUIRED COMPONENTS
  ITKCommon
  ITKIOImageBase
  ITKImageFunction
  ITKVTK
  ${ITK_IO_MODULES_USED}
  )
include(${ITK_USE_FILE})

# Find VTK
if(NOT VTK_RENDERING_BACKEND)
  set(VTK_RENDERING_BACKEND OpenGL)
endif()
find_package(VTK REQUIRED NO_MODULE COMPONENTS
  vtkCommonCore
  vtkCommonDataModel
  vtkFiltersSources
  vtkIOCore
  vtkIOLegacy
  vtkIOXML
  vtkRenderingCore
  vtkRendering${VTK_RENDERING_BACKEND}
)
set( VTK_VERSION_REQUIRED 6 )
if( VTK_VERSION_MAJOR LESS ${VTK_VERSION_REQUIRED} )
  message( FATAL_ERROR "${PROJECT_NAME} requires VTK ${VTK_VERSION_REQUIRED}. Version Found: ${VTK_VERSION_MAJOR}" )
endif()
include(${VTK_USE_FILE})

SEMMacroBuildCLI(
  NAME PolyDataCompression
  EXECUTABLE_ONLY
  TARGET_LIBRARIES ${ITK_LIBRARIES} ${VTK_LIBRARIES}
  )
include(ExternalData)

option(BUILD_TESTING "Build the testing tree" ON)

if(BUILD_TESTING)
  include(CTest)
  add_subdirectory(Testing)
endif()
