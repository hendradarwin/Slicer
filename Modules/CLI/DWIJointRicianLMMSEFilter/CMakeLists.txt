
#-----------------------------------------------------------------------------
set(MODULE_NAME DWIJointRicianLMMSEFilter)

#-----------------------------------------------------------------------------

#
# SlicerExecutionModel
#
find_package(SlicerExecutionModel REQUIRED)
include(${SlicerExecutionModel_USE_FILE})

#
# ITK
#
set(${PROJECT_NAME}_ITK_COMPONENTS
  ITKCommon
  ITKIOImageBase
  ITKIONRRD
  ITKImageFilterBase
  ${ModuleDescriptionParser_ITK_COMPONENTS}
  )
find_package(ITK 4.8 COMPONENTS ${${PROJECT_NAME}_ITK_COMPONENTS} REQUIRED)
set(ITK_NO_IO_FACTORY_REGISTER_MANAGER 1) # See Libs/ITKFactoryRegistration/CMakeLists.txt
include(${ITK_USE_FILE})
message(STATUS "finding ITK component for project: ${PROJECT_NAME} , itk component is: ${${PROJECT_NAME}_ITK_COMPONENTS} ")


#-----------------------------------------------------------------------------
set(${MODULE_NAME}_TARGET_LIBRARIES
  ${ITK_LIBRARIES} ModuleDescriptionParser
  )

#-----------------------------------------------------------------------------
SEMMacroBuildCLI(
  NAME ${MODULE_NAME}
  LOGO_HEADER ${Slicer_SOURCE_DIR}/Resources/NAMICLogo.h
  TARGET_LIBRARIES ${${MODULE_NAME}_TARGET_LIBRARIES}
  INCLUDE_DIRECTORIES
    ${SlicerBaseCLI_SOURCE_DIR} ${SlicerBaseCLI_BINARY_DIR}
  EXECUTABLE_ONLY
  )

#-----------------------------------------------------------------------------
if(BUILD_TESTING)
  add_subdirectory(Testing)
endif()
